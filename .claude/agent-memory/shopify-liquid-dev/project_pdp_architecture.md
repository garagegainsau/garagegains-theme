---
name: PDP Architecture
description: How the GarageGains product detail page is structured — section order, layout pattern, and inter-section communication
type: project
---

## Section order in templates/product.json

1. `gg-product-gallery` — sticky vertical thumbnail strip + main image, badge overlay, video support
2. `gg-product-info` — breadcrumbs, title, star rating (metafield), price/savings, SKU, short desc
3. `gg-product-purchase` — variant pills (skewed), quantity, availability indicator, Ajax ATC, wishlist
4. `gg-product-trust` — 3 trust-signal rows with inline SVG icons, title, subtitle
5. `gg-product-tabs` — Description / Tech Specs / Shipping tabs (CSS radio-input hack, no JS)
6. `gg-upsell` — "COMPLETE YOUR SETUP" recommendations via /recommendations/products.json

## Layout note

The gallery (1) and the info/purchase column (2–4) are laid out side-by-side in the theme wrapper,
not within these sections themselves. These sections render as stacked blocks; the two-column PDP
grid is handled by a wrapper in layout/theme.liquid or a parent section context.

## Inter-section event bus

`gg-product-purchase` dispatches `document.dispatchEvent(new CustomEvent('variant:change', { detail: { variant } }))`.
Both `gg-product-gallery` and `gg-product-info` listen for this event to sync the active image and price display.

## Recommendations endpoint

`gg-upsell` fetches: `/recommendations/products.json?product_id={{ product.id }}&limit=N&intent=related`
Requires at least 10 published products in the store before Shopify returns results.
Falls back silently (section hidden) on fetch error or empty results.

**Why:** Clean event-driven approach avoids tight coupling between sections while keeping each
section independently editable in the Theme Editor.
**How to apply:** Any new PDP section needing variant awareness should listen for `variant:change`
on document rather than querying sibling sections directly.
