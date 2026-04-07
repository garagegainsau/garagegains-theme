---
name: Section Build Status
description: Tracks which GarageGains sections are complete vs. still to build, as of 2026-04-06
type: project
---

## PDP Sections (v2 — rebuilt 2026-04-06)

| File | Status |
|---|---|
| `sections/gg-product-main.liquid` | COMPLETE v2 — 2-column combined section (gallery + info + buy box + trust + delivery accordion) |
| `sections/gg-product-tabs.liquid` | COMPLETE v2 — below-fold accordions (Description, Tech Specs, Shipping) using `<details>/<summary>` |
| `sections/gg-upsell.liquid` | REMOVED from product.json — replaced by gg-pair-up + gg-discover-more |
| `sections/gg-pair-up.liquid` | COMPLETE — "PAIR UP WITH" bundle section: current product + 2 companions, checkboxes, running total, batch ATC |
| `sections/gg-discover-more.liquid` | COMPLETE — "DISCOVER MORE" two-tab carousel: Tab 1 Liquid brand collection, Tab 2 JS recommendations, CSS scroll-snap arrows |
| `templates/product.json` | COMPLETE v3 — order: gg-product-main → gg-product-tabs → gg-pair-up → gg-discover-more |

**Architecture change from v1:** v2 consolidates the 2-col layout (gallery left + info/purchase/trust right)
inside a single `gg-product-main.liquid` section, matching how Dawn's `main-product.liquid` works.
v1 tried to split these across 4 separate sections which cannot share a CSS grid.

**Reference:** Merchant approved Rebel Sport layout as reference (2026-04-06):
`website references/pdp references/rebel.html` + `rebel pdp main reference.png`

## Homepage Sections

| File | Status |
|---|---|
| `sections/gg-hero.liquid` | COMPLETE — full-viewport hero with Ken Burns, stats row, scroll indicator |
| `sections/gg-trust-bar.liquid` | To build |
| `sections/gg-ticker.liquid` | To build |
| `sections/gg-categories.liquid` | To build |
| `sections/gg-featured-products.liquid` | To build |
| `sections/gg-email-cta.liquid` | COMPLETE |
| `sections/gg-trust-badges.liquid` | To build |
| `sections/gg-reviews.liquid` | To build |

## PLP Sections

| File | Status |
|---|---|
| `sections/gg-collection-header.liquid` | To build |

## Global / Layout Sections

| File | Status |
|---|---|
| `sections/footer.liquid` | COMPLETE — full GarageGains rewrite. 4-col grid (brand / 2x link_list / newsletter), social icons, payment icons, policy links, ember accent line. Autofill flash fix applied. |

**Why:** PDP v2 built after merchant provided Rebel Sport as reference layout. Single combined section approach
avoids Shopify's inability to share CSS grid across `shopify-section` wrappers.
**How to apply:** Any further PDP additions go inside `gg-product-main.liquid` right column, or as their own
full-width section below it (like tabs and upsell already are).
