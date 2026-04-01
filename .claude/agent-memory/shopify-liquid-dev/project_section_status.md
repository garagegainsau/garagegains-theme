---
name: Section Build Status
description: Tracks which GarageGains sections are complete vs. still to build, as of 2026-04-01
type: project
---

## PDP Sections

| File | Status |
|---|---|
| `sections/gg-product-gallery.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `sections/gg-product-info.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `sections/gg-product-purchase.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `sections/gg-product-trust.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `sections/gg-product-tabs.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `sections/gg-upsell.liquid` | REVERTED — merchant rejected design 2026-04-01 |
| `templates/product.json` | REVERTED to Dawn default — awaiting approved PDP rebuild |

**See `feedback_pdp_aesthetic.md` before attempting any PDP rebuild.**

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

**Why:** PDP was the first major build target but was rejected by merchant — all 6 files and product.json reverted. Homepage sections are next in CLAUDE.md order.
**How to apply:** Do not rebuild PDP without first reading `feedback_pdp_aesthetic.md` and getting explicit merchant direction on look/feel.
