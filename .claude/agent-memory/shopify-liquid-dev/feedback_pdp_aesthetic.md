---
name: PDP Aesthetic Rejection
description: Merchant rejected the first PDP build (all 6 sections + product.json) — do not repeat this aesthetic approach
type: feedback
---

The entire first PDP build was rejected by the merchant on 2026-04-01. All 6 section files (`gg-product-gallery.liquid`, `gg-product-info.liquid`, `gg-product-purchase.liquid`, `gg-product-trust.liquid`, `gg-product-tabs.liquid`, `gg-upsell.liquid`) and `templates/product.json` were reverted.

**Why:** The design did not suit the brand or the products being sold (commercial gym equipment). The aesthetic was not aligned with GarageGains' Raw Industrial Premium direction.

**How to apply:** Before any PDP rebuild, get explicit merchant sign-off on a design direction. Do not proceed with a full section build based only on CLAUDE.md specs — show a concept or ask what specifically they want to see first. The product page must feel premium, industrial, and purpose-built for heavy gym equipment — not like a generic eCommerce template.

Key things to avoid repeating (inferred from rejection):
- Overly generic card/grid layouts that could belong to any store
- Any light-coloured surfaces or non-dark-mode elements on the PDP
- Layout structures that feel more suited to apparel/lifestyle than heavy equipment
- Anything that doesn't feel like it belongs alongside the hero and footer aesthetic already approved

## PDP v2 — Built 2026-04-06

The merchant provided reference files in `website references/pdp references/` including `rebel.html`
and `rebel pdp main reference.png`. A full detailed brief was provided specifying:
- Rebel Sport layout pattern (2-col grid, image grid left, buy box right, accordions below)
- GarageGains dark treatment applied throughout (no light surfaces)
- Single combined section `gg-product-main.liquid` to handle the 2-col layout correctly
- `<details>/<summary>` accordions (no JS) for Description / Tech Specs / Shipping
- Upsell grid with shimmer skeletons and silent JS fallback

v2 files: `sections/gg-product-main.liquid`, `sections/gg-product-tabs.liquid`,
`sections/gg-upsell.liquid`, `templates/product.json` (all written to `feature/pdp-v2` branch).
