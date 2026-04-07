---
name: PDP v2 Audit Findings
description: Critical bugs and warnings found during the 2026-04-06 audit of gg-product-main, gg-product-tabs, gg-upsell, product.json, and garagegains.css
type: project
---

## Audit date: 2026-04-06
## Files audited: gg-product-main.liquid, gg-product-tabs.liquid, gg-upsell.liquid, product.json, garagegains.css (lines 1100+)

---

## Critical Issues Fixed

### 1. Liquid operator precedence bug — gg-product-main.liquid
**Location:** Star rating loop, `elsif` condition
**Bug:** `{%- elsif half_star and i == full_stars | plus: 1 -%}` — the `| plus: 1` filter is applied to the entire comparison expression in Liquid, not just to `full_stars`. This produces unpredictable results for non-integer ratings.
**Fix:** Pre-assign `{%- assign half_star_position = full_stars | plus: 1 -%}` before the loop, then compare `i == half_star_position`. Never chain a filter directly onto a comparison RHS — always pre-assign to a variable.

### 2. Invalid parameter on `form` tag — gg-product-main.liquid
**Location:** Line 217, `{%- form 'product', product, ..., data-type: 'add-to-cart-form' -%}`
**Bug:** `data-*` attributes are not valid parameters for the Liquid `form` tag. Passing `data-type:` causes a Liquid render error or silent drop depending on Shopify version. The `form` tag only accepts: `id`, `class`, `novalidate`, `return_to`.
**Fix:** Removed `data-type: 'add-to-cart-form'` from the form tag. If a data attribute on the `<form>` element is needed for JS targeting, add it as a raw HTML attribute on the rendered `<form>` — but the existing JS already targets by `id`, so no replacement was needed.

### 3. Upsell images not serving WebP — gg-upsell.liquid
**Location:** JS image URL construction in the recommendations fetch handler
**Bug:** Used the legacy Shopify CDN size-suffix pattern `image.src.replace(/(\.[^.]+)$/, '_800x800_crop_center$1')` — this serves JPG/PNG, violating the WebP-only rule.
**Fix:** Replaced with `image.src.replace(/(_\d+x\d*(_crop_\w+)?)?(\?.*)?$/, '') + '?width=800&format=webp'` — strips any existing size suffix and query string, then appends Shopify CDN query parameters for width and WebP format. This is the correct client-side approach when `image_url` Liquid filter is unavailable (JS context).

### 4. Off-palette colour on star ratings — garagegains.css
**Location:** `.gg-pdp-stars { color: #F5A623 }`
**Bug:** `#F5A623` (amber/gold) is not in the GarageGains colour palette. Per CLAUDE.md, star ratings use Ember (`#C94A1E`).
**Fix:** Replaced with `color: var(--ember)`.

---

## Warnings (not auto-fixed)

### W1. Shimmer animation uses `background-position` — garagegains.css
**Location:** `@keyframes gg-shimmer` (line ~1790)
CLAUDE.md states "CSS animations use transform and opacity only". The shimmer animates `background-position` on a `background-size: 200%` gradient. Strictly this violates the letter of the rule, but `background-position` on a pre-sized gradient is a well-established, GPU-friendly pattern for skeleton loaders — all major frameworks use it. The banned properties (`width`, `height`, `top`, `left`) cause layout recalculation; `background-position` does not. Recommend confirming with the team whether to exempt shimmer from the rule or refactor to a `::after` pseudo-element with `transform: translateX()`.

### W2. `data-type` attribute on form removed — JS selector impact
The removed `data-type="add-to-cart-form"` attribute is not referenced anywhere in the current JS, so no functional regression. However, if Dawn's theme editor JS or any third-party app uses this attribute as a hook, it will no longer be present. Verify no dependencies before marking closed.

### W3. `imgSrcset` variable is unused — gg-upsell.liquid
**Location:** Line 79, `const imgSrcset = imageUrl`
The variable `imgSrcset` is assigned but the card HTML on line 83 uses `imageUrl` directly (`src="' + imgSrcset + '"`). Actually `imgSrcset` IS used — but it is identical to `imageUrl` making it a pointless alias. Not a bug, but dead code. Could be cleaned up.

---

## Patterns to Avoid in Future Builds

1. **Never filter on a comparison RHS in Liquid.** Always pre-assign: `{%- assign val = x | plus: 1 -%}` then use `val` in the condition. Liquid's filter chain binds tightly to the left operand in some Shopify versions but ambiguously in `elsif` conditions.

2. **`form` tag only accepts: `id`, `class`, `novalidate`, `return_to`.** Any other key-value pair passed to the Liquid `form` tag is invalid. Data attributes belong on the rendered HTML element, not in the tag parameters — and since Shopify controls the `<form>` open tag, you cannot add raw `data-*` attributes to it. Use the form's `id` for JS targeting instead.

3. **JS image URL construction must still serve WebP.** When building product card HTML in JavaScript (e.g., recommendations API responses), use Shopify CDN query params: strip the URL back to the base path then append `?width=NNN&format=webp`. Do not use the legacy `_NNNxNNN` suffix pattern.

4. **All colours must reference palette tokens.** Never use a hex code that is not in the GarageGains palette (`#111111`, `#1E1E1E`, `#5C5C5C`, `#E2DED8`, `#F5F3EF`, `#C94A1E`). Use `var(--ember)` etc. rather than hardcoded hex in CSS. The off-palette `#F5A623` gold crept in for stars — this is a common mistake when referencing external examples.

5. **`image_tag` named params cannot use inline filter chains.** `alt: image.alt | escape` inside `image_tag` named parameters is invalid Shopify Liquid and causes the section to be silently rejected. Always pre-assign: `{%- assign img_alt = image.alt | escape -%}` then pass `alt: img_alt`.

6. **Range schema fields require integer `step` values.** Shopify rejects any range field where `"step"` is a float (e.g. `0.5`). Use `"step": 1` always. If fractional granularity is needed, use `"type": "text"` and parse in Liquid instead.

7. **`shopify theme dev` does NOT reliably push new section files.** Hot-reload only syncs file changes it detects after start-up. If a section was added during development (never previously existed on Shopify's servers) and upload failed silently, `dev` mode will not retry it. The cascade error "Section type 'X' does not refer to an existing section file" in a template JSON is always caused by the section file not existing on Shopify's servers — either it was never pushed or a previous upload was rejected. Fix: `shopify theme push --theme [ID] --only sections/[file].liquid templates/[template].json`. Get the theme ID first via `shopify theme list`.

8. **Use `shopify theme check` to validate before pushing.** Running `shopify theme check --path .` locally catches schema and Liquid errors without touching the remote theme. The error output is per-file; grep for the specific section name to filter results.

---

## Files That Passed Clean

- **gg-product-tabs.liquid** — No issues. Liquid tags correctly opened/closed, schema valid, `presets` present, no hardcoded stock, no render-blocking JS (no JS at all), `<details>/<summary>` accordion pattern clean.
- **product.json** — Valid JSON, section `type` values match filenames exactly, `block_order` matches `blocks` keys, no trailing commas.
- **gg-product-main.liquid (JS)** — `const`/`let` only, no `var`, no `console.log`, no inline `onclick`, ATC posts to `/cart/add.js` with error handling, `type="module"` script tag.
- **garagegains.css (PDP section)** — No `width`/`height`/`top`/`left` animations, no duplicate class definitions.

**Why:** Saves time in future audits by knowing which patterns are risky vs. which were solid.
**How to apply:** In future PDP section builds, treat the form tag parameters, Liquid filter chaining on comparisons, and JS image URL construction as the three highest-risk areas.
