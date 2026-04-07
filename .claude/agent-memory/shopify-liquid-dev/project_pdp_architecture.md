---
name: PDP Architecture
description: How the GarageGains PDP v2 is structured — single combined section for 2-col layout, accordions below, upsell at bottom
type: project
---

## v2 Architecture (2026-04-06)

### Section files and product.json order

1. `gg-product-main.liquid` — The entire 2-column PDP: image grid (left) + info/buy box (right)
2. `gg-product-tabs.liquid` — Full-width below-fold: Description / Tech Specs / Shipping accordions
3. `gg-upsell.liquid` — "COMPLETE YOUR SETUP" related products grid

### Why single section for 2-col layout

Shopify wraps each section in its own `<div class="shopify-section">`. CSS grid cannot span across
those wrapper divs. The only correct approach for a 2-column PDP in Shopify OS 2.0 is to put both
columns inside one section — exactly how Dawn's `main-product.liquid` works. Do NOT split gallery
and info into separate sections expecting a shared grid to work.

### Left column (gallery)
- Desktop: 2-col image grid. First image spans full width (`grid-column: 1 / -1`), rest fill 2-col grid
- Mobile (≤900px): horizontal snap-scroll strip — CSS only, no JS
- Sticky on desktop (`position: sticky; top: 100px`) — clears the floating nav bar
- White background tiles (`background: #f8f8f8`) so product shots pop against the dark theme
- First image: `loading="eager" fetchpriority="high"`. All others: `loading="lazy"`

### Right column (info + buy box)
- Breadcrumbs → Title (H1 Barlow Condensed uppercase) → Stars → Price block → SKU → Short desc
- Variant pills (`.gg-variant-pill .gg-btn`) — skewed; active = ember fill, unavailable = 30% opacity + disabled
- Quantity control — no skew (utility element), 36×40px minus/plus flanking a text input
- ATC: full-width `.gg-btn.gg-btn--primary.gg-pdp-atc`, 56px height
- Wishlist: ghost link below ATC
- Trust row: 3-col grid of icon + title + subtitle items (SVG icons inline)
- Delivery `<details>/<summary>` accordion below trust row

### Accordions (`gg-product-tabs.liquid`)
- `<details>/<summary>` — zero JS. `open` attribute on Description by default.
- Summary uses CSS `::after` for +/− toggle; `summary::marker` and `::-webkit-details-marker` both suppressed
- Tech Specs renders only if at least one block has a non-blank `spec_label`
- Shipping falls back to default hardcoded copy if richtext field is blank

### Upsell (`gg-upsell.liquid`)
- Shopify recommendations API: `/recommendations/products.json?product_id=X&limit=N&intent=related`
- Section hidden (`display:none`) at render time; JS shows it after successful fetch
- Skeleton shimmer cards visible while fetching (CSS `gg-shimmer` keyframe animation)
- Silent fail: section stays hidden if API errors or returns 0 products
- Requires 10+ published products in store before Shopify returns recommendations

### Inter-section events
- `gg-product-main.liquid` dispatches `variant:change` CustomEvent on `document` when a variant pill is clicked
- Any future section that needs variant awareness should listen for `variant:change` on `document`
- ATC success dispatches `cart:updated` on `document` for cart drawer to listen to

### CSS classes (all in assets/garagegains.css, PDP section)
Key grid classes: `.gg-pdp-wrapper`, `.gg-pdp-gallery`, `.gg-pdp-info`, `.gg-pdp-image-grid`,
`.gg-pdp-image-grid__item`, `.gg-pdp-image-grid__item--hero`, `.gg-pdp-mobile-strip`
Key component classes: `.gg-pdp-title`, `.gg-pdp-price-now`, `.gg-pdp-price-was`, `.gg-pdp-save-badge`,
`.gg-pdp-trust`, `.gg-pdp-delivery`, `.gg-pdp-accordions`, `.gg-pdp-accordion`, `.gg-richtext`,
`.gg-specs-table`, `.gg-shimmer`

**Why:** Consolidated section approach eliminates the v1 mistake of trying to span a CSS grid across
multiple Shopify section wrappers.
**How to apply:** If merchant ever wants to add a new PDP component that is part of the 2-col layout
(e.g. a bundle builder), it must go inside `gg-product-main.liquid`. Full-width components below
the fold can be independent sections added to product.json.
