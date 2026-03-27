# CLAUDE.md — GarageGains Shopify Storefront

This file is the single source of truth for Claude Code when building the GarageGains Shopify storefront. Read this file in full before writing any code, generating any section, or making any design decision.

---

## 1. Project Overview

| Field | Value |
| :--- | :--- |
| **Brand** | GarageGains (Australia) |
| **Core Phrase** | REDEFINING PERFORMANCE |
| **Mission** | Commercial-grade gym equipment without the commercial price tag — built for hard-training everyday Australians |
| **Aesthetic** | Raw Industrial Premium — dark-mode, high-contrast, gritty, dramatic |
| **Platform** | Shopify Online Store 2.0 |
| **Base Theme** | Dawn (forked from `github.com/Shopify/dawn`) |
| **Supplier** | Morgan Sports Australia (`morgansports.com.au`) — B2B wholesale, est. 1988 |

---

## 2. Dev Stack & Environment

### 2.1 Confirmed Environment
- **OS:** Windows 10
- **Node.js:** v24.14.1
- **npm:** v11.11.0
- **Shell:** bash (use Unix-style paths and commands)

### 2.2 Required Tools

| Tool | Install Command | Purpose |
| :--- | :--- | :--- |
| Shopify CLI | `npm install -g @shopify/cli@latest` | Local dev server, theme push/pull |
| Tailwind CSS v4 | `npm install tailwindcss @tailwindcss/postcss postcss` | Utility-first CSS |
| PostCSS | included above | CSS build pipeline |

### 2.3 One-Time Setup (run in project root after cloning Dawn)

```bash
# 1. Install Shopify CLI globally
npm install -g @shopify/cli@latest

# 2. Install theme dependencies
npm install tailwindcss @tailwindcss/postcss postcss

# 3. Connect to Shopify store (replace with actual store URL)
shopify theme dev --store garagegains-australia.myshopify.com
```

### 2.4 Daily Development Commands

```bash
# Start local dev server with hot reload
shopify theme dev --store garagegains-australia.myshopify.com

# Push all local changes to store
shopify theme push

# Pull latest theme from store
shopify theme pull

# Push to a specific theme (by ID)
shopify theme push --theme [THEME_ID]
```

### 2.5 Tailwind Build Config

**`postcss.config.mjs`** (create in project root):
```js
export default {
  plugins: {
    "@tailwindcss/postcss": {},
  }
}
```

**Main CSS entry** (`assets/garagegains.css`):
```css
@import "tailwindcss";
```

Reference `garagegains.css` in `layout/theme.liquid` via:
```liquid
{{ 'garagegains.css' | asset_url | stylesheet_tag }}
```

---

## 3. Theme Architecture (Shopify OS 2.0 / Dawn)

### 3.1 Folder Structure

```
dawn/
├── assets/          ← CSS, JS, images, SVGs, WebP files
├── blocks/          ← Reusable app blocks
├── config/          ← settings_schema.json (Theme Editor global settings)
├── layout/          ← theme.liquid (wraps everything), password.liquid
├── locales/         ← Translation strings (en.default.json etc.)
├── sections/        ← Custom & Dawn sections (.liquid files with {% schema %})
├── snippets/        ← Reusable Liquid fragments (no schema, not in editor)
└── templates/       ← JSON templates (index.json, product.json, etc.)
```

### 3.2 Key Concepts

- **Sections** — Modular Liquid files with `{% schema %}` blocks. Merchants can add/reorder them in the Theme Editor. All GarageGains custom sections go here.
- **Snippets** — Reusable Liquid fragments rendered via `{% render 'snippet-name' %}`. No schema, not visible in Theme Editor.
- **JSON Templates** — e.g. `templates/index.json`. Define which sections appear on a page and their default order.
- **`{% schema %}`** — JSON block inside each section defining Theme Editor settings (text, image, color, select fields, blocks). Always include a schema so merchants can edit content without code.
- **Settings** — Global theme settings defined in `config/settings_schema.json`, accessed via `{{ settings.variable_name }}`.

### 3.3 How to Create a New Custom Section

1. Create `sections/gg-[section-name].liquid`
2. Write HTML/Liquid markup using GarageGains design system (see Section 4)
3. Add `{% schema %}` block at the bottom defining all editable fields
4. Add the section to the relevant JSON template (e.g. `templates/index.json`)

---

## 4. Design System — Apply to Every Component

### 4.1 Colour Palette

| Token | Hex | Usage |
| :--- | :--- | :--- |
| `forge-black` | `#111111` | Primary background, heavy structural elements |
| `iron` | `#1E1E1E` | Cards, section backgrounds, secondary surfaces |
| `steel` | `#5C5C5C` | Dividers, secondary UI, "NEW" badges |
| `ash` | `#E2DED8` | Metadata, subtext, secondary labels |
| `bone` | `#F5F3EF` | Primary body text (high contrast on dark) |
| `ember` | `#C94A1E` | CTAs, sale badges, star ratings, accents — PRIMARY ACTION COLOUR |

**In Tailwind**, use arbitrary value syntax:
```html
<div class="bg-[#111111] text-[#F5F3EF]">
<button class="bg-[#C94A1E] text-[#111111]">
```

### 4.2 Typography

| Use | Font | Weight | Style |
| :--- | :--- | :--- | :--- |
| Display / Headlines | Barlow Condensed | ExtraBold (800) / Black (900) | ALL-CAPS always |
| Body / UI / Nav | DM Sans | Regular (400) / Medium (500) | Sentence case |

**Load via Google Fonts** in `layout/theme.liquid`:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@800;900&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
```

**Headline sizing** must use `clamp()` for fluid scaling:
```css
font-size: clamp(2rem, 5vw, 5rem);
```

### 4.3 The "Performance Skew" — MANDATORY UI MOTIF

All buttons, CTA labels, and product badges must use a 20-degree skewed parallelogram shape.

```css
/* Base skew class — apply to all CTAs and badges */
.gg-skew {
  display: inline-block;
  transform: skewX(-20deg);
  padding: 0.75rem 2rem;
}

/* Counter-skew the inner text so it reads straight */
.gg-skew span {
  display: inline-block;
  transform: skewX(20deg);
}
```

```html
<!-- Example CTA button -->
<a href="/collections/all" class="gg-skew bg-[#C94A1E]">
  <span class="font-['Barlow_Condensed'] font-black text-[#111111] uppercase tracking-widest">
    SHOP NOW
  </span>
</a>
```

### 4.4 Responsive Breakpoints

| Breakpoint | Range | Notes |
| :--- | :--- | :--- |
| Mobile | up to 809px | Mobile-first default. Hamburger nav. |
| Tablet | 810px – 1439px | Mid-range grid adjustments. |
| Desktop | 1440px+ | Full layout. Horizontal nav. |

```css
/* Tailwind custom breakpoints — add to CSS config */
@theme {
  --breakpoint-tablet: 810px;
  --breakpoint-desktop: 1440px;
}
```

### 4.5 Imagery Rules

- All images must be served in **WebP format**
- All images below the fold must have `loading="lazy"`
- Photography must be desaturated/high-contrast — apply via CSS filter if needed:
  ```css
  filter: grayscale(30%) contrast(1.1);
  ```
- Product shots on product cards use **white backgrounds** to pop against dark theme

---

## 5. Homepage Sections (Build in This Order)

Each section maps to a file in `sections/`. All sections must be registered in `templates/index.json`.

### Section 1 — Full-Bleed Hero (`gg-hero.liquid`)
- Full-width background image with 40% dark overlay gradient
- Left-aligned headline in Barlow Condensed, ALL-CAPS, `clamp()` sizing
- Two skewed CTA buttons: `[SHOP NOW]` (Ember fill) and `[EXPLORE GEAR]` (outlined)
- Schema fields: `heading`, `subheading`, `image`, `cta_1_label`, `cta_1_url`, `cta_2_label`, `cta_2_url`

### Section 2 — Trust Bar (`gg-trust-bar.liquid`)
- Full-width row, Iron background (`#1E1E1E`)
- 4 icon + label items: **Free Delivery**, **30-Day Returns**, **Lifetime Warranty**, **Secure Payment**
- Icons: Steel-coloured SVGs inline in snippet
- Responsive: horizontal row on desktop, 2x2 grid on mobile
- Schema: each item has `icon`, `label`, `subtext` fields

### Section 3 — Scrolling Ticker (`gg-ticker.liquid`)
- CSS `@keyframes` marquee loop — no JavaScript
- Ember background (`#C94A1E`), Forge Black text
- Content: `RACKS • BARBELLS • PLATES • BENCHES • DUMBBELLS • FLOORING •`
- Duplicated content block for seamless infinite loop
- Schema: `ticker_text`, `speed` (slow/medium/fast)

### Section 4 — Shop By Category (`gg-categories.liquid`)
- 3-column grid (6 category cards) on desktop → 2-column on tablet → 1-column on mobile
- Each card: full-bleed athlete image, category name in Barlow Condensed, optional Ember "BEST SELLER" badge
- Emoji fallback if no image is set (e.g. 🏋️ for Barbells)
- Schema: 6 blocks, each with `image`, `title`, `link`, `show_badge`, `badge_label`

### Section 5 — Featured Gear (`gg-featured-products.liquid`)
- 4-up product grid pulling from a selected Shopify collection
- White-background product images to contrast against Iron card backgrounds
- Product card: image, title, price, SALE badge (Ember), NEW badge (Steel), BEST SELLER badge (Ember) — driven by product tags
- "ADD TO CART" button uses skew motif
- Schema: `collection`, `heading`, `products_to_show` (default 4)

### Section 6 — Email CTA (`gg-email-cta.liquid`)
- Iron background, generous vertical padding
- Large Ember headline: "10% OFF YOUR FIRST ORDER"
- Subtext in Ash, email input + skewed submit button
- Connects to Shopify's built-in newsletter customer tag
- Schema: `heading`, `subtext`, `button_label`

### Section 7 — Trust Badges (`gg-trust-badges.liquid`)
- 4-up grid on Iron background
- Items: "Built for Australians," "3mm Steel Gauge," "Direct to Gym," "Commercial Grade"
- Industrial/blueprint style SVG icons (Bone coloured)
- Schema: 4 blocks with `icon`, `title`, `description`

### Section 8 — Customer Reviews (`gg-reviews.liquid`)
- 3-column layout on Iron card backgrounds
- Star ratings rendered in Ember (`#C94A1E`)
- Responsive: 3-col desktop → 1-col mobile (horizontal scroll or stack)
- Fully editable via Theme Editor
- Schema: up to 6 review blocks, each with `author`, `rating` (1–5), `review_text`, `location`

---

## 6. Collection Page (PLP) — `sections/gg-collection-header.liquid`

- Dynamic hero banner: collection `title` and `description` on Iron background
- Filter bar: Price, Weight, Steel Gauge — use Shopify's native `predictive_search` or filter API
- Sort By dropdown (Shopify native)
- Product count display: "Showing X products"
- Grid: 4 cols (desktop) → 3 cols (tablet) → 2 cols (mobile)
- Automated badges from product tags:
  - Tag `sale` → Ember "SALE" badge
  - Tag `new` → Steel "NEW" badge
  - Tag `best-seller` → Ember "BEST SELLER" badge

---

## 7. Product Page (PDP) — `templates/product.json`

Build as a modular set of sections added to `templates/product.json`:

| Section File | Purpose |
| :--- | :--- |
| `gg-product-gallery.liquid` | Main image + vertical thumbnail switcher |
| `gg-product-info.liquid` | Breadcrumbs, title, star rating, price, sale savings |
| `gg-product-purchase.liquid` | Variant selectors (skewed pills), Add to Cart (Ajax), Wishlist |
| `gg-product-trust.liquid` | 3 trust rows: Ships from Melbourne, Lifetime Warranty, etc. |
| `gg-product-tabs.liquid` | Tabbed UI: Description / Tech Specs / Shipping |
| `gg-upsell.liquid` | "Complete Your Setup" related products grid |

### Variant Selectors
Use skewed parallelogram pill style (same `.gg-skew` class, smaller padding).

### Ajax Cart
Update the Shopify cart without page reload. POST to `/cart/add.js`, then update the sidebar cart drawer count.

### Tech Specs Tab
Structured data fields for: Steel Gauge, Load Rating, Dimensions (L x W x H), Weight, Finish. These values come from Morgan Sports product specifications.

---

## 8. Navigation

- **Desktop:** Horizontal nav links in `layout/theme.liquid` header. Font: DM Sans Medium. Colour: Bone. Hover: Ember underline.
- **Mobile/Tablet:** Slide-out hamburger menu from the left. Full-screen overlay, Forge Black background. Links in Barlow Condensed.
- **Sticky header** on scroll with slight Iron background and box-shadow for depth.

---

## 9. Performance Rules (Non-Negotiable)

- All images: **WebP only**. Use Shopify's `| image_url` filter with `format: 'webp'`
  ```liquid
  {{ image | image_url: width: 800, format: 'webp' | image_tag: loading: 'lazy' }}
  ```
- All images below the fold: `loading="lazy"`
- Hero image above fold: `loading="eager"`, `fetchpriority="high"`
- No render-blocking JS. Use `defer` or `type="module"` on all script tags
- CSS animations (ticker, transitions): use `transform` and `opacity` only — never animate `width`, `height`, or `top/left`
- Tailwind: purge unused classes via PostCSS build (only ship what's used)

---

## 10. Supplier Context (Morgan Sports)

- **Supplier:** Morgan Sports Australia — B2B wholesale only, est. 1988
- **Key Trust Signal:** Same wholesaler behind Snap Fitness, F45, World Gym, UFC Gym
- **Product data source:** All titles, descriptions, specs, and images originate from the Morgan Sports wholesale catalogue
- **Credibility copy to use:** *"Commercial-grade gear. No commercial markup."* and *"The same equipment trusted by Australia's leading gym chains."*
- **Stock note:** Never hardcode "In Stock" — always use Shopify's `product.available` and `variant.available` Liquid variables

---

## 11. Claude Code Conventions — Read Before Writing Any Code

1. **Always write Shopify Liquid** for sections and snippets — not plain HTML files
2. **Every section must have a `{% schema %}`** block so merchants can edit it in the Theme Editor
3. **Use Tailwind utility classes** as the primary styling method. Write custom CSS only for the skew motif, marquee animation, and `clamp()` typography
4. **Mobile-first always** — write base styles for mobile, then use `md:` and `lg:` Tailwind breakpoints for larger screens
5. **Never use white or light backgrounds as a base** — all surfaces default to Forge Black or Iron
6. **All headlines are ALL-CAPS Barlow Condensed** — never use sentence case for display text
7. **All CTAs use the Performance Skew** — never use standard rectangular buttons
8. **Image output must include WebP format and lazy loading** — use the Shopify `image_url` filter
9. **Comment section boundaries** — start each section file with a comment block:
   ```liquid
   {% comment %}
     Section: GG Hero
     File: sections/gg-hero.liquid
     Description: Full-bleed homepage hero with skewed CTAs
   {% endcomment %}
   ```
10. **Prefix all custom section files with `gg-`** to distinguish from Dawn's native sections

---

## 12. GitHub Workflow

```
main         ← production-ready code, mirrors live theme
develop      ← integration branch, all features merge here
feature/*    ← individual section or feature branches
```

- Branch naming: `feature/gg-hero`, `feature/gg-ticker`, `feature/pdp-tabs`
- Never push directly to `main`
- Use `shopify theme push --theme [THEME_ID]` to push to a development theme for testing before merging to `main`
- Keep Dawn upstream remote: `git remote add upstream https://github.com/Shopify/dawn.git`
- Pull Dawn updates: `git fetch upstream && git merge upstream/main` (test for conflicts with custom sections)

---

## 13. Quick Reference — GarageGains Section Files

| File | Homepage Order | Status |
| :--- | :--- | :--- |
| `sections/gg-hero.liquid` | 1 | To build |
| `sections/gg-trust-bar.liquid` | 2 | To build |
| `sections/gg-ticker.liquid` | 3 | To build |
| `sections/gg-categories.liquid` | 4 | To build |
| `sections/gg-featured-products.liquid` | 5 | To build |
| `sections/gg-email-cta.liquid` | 6 | To build |
| `sections/gg-trust-badges.liquid` | 7 | To build |
| `sections/gg-reviews.liquid` | 8 | To build |
| `sections/gg-collection-header.liquid` | PLP | To build |
| `sections/gg-product-gallery.liquid` | PDP | To build |
| `sections/gg-product-info.liquid` | PDP | To build |
| `sections/gg-product-purchase.liquid` | PDP | To build |
| `sections/gg-product-trust.liquid` | PDP | To build |
| `sections/gg-product-tabs.liquid` | PDP | To build |
| `sections/gg-upsell.liquid` | PDP | To build |
