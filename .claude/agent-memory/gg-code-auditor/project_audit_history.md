---
name: Section audit history
description: Record of sections audited, their final status, and key findings
type: project
---

## sections/gg-header.liquid
- **Audited:** 2026-04-09
- **Status:** PASS WITH WARNINGS (2 Critical, 4 Warnings)
- **Version audited:** Full rewrite — floating pill nav replaced with full-width sticky nav, mega menu dropdowns, mobile accordion drawer
- **Critical C2:** `.gg-mega { z-index: 199 }` renders behind `.gg-nav { z-index: 200 }` — fix to `z-index: 201`
- **Critical C3:** `fetch('/cart.js')` in cart:updated listener has no `.catch()` — add error handler
- **Key warnings:** `padding-left` animated on hover (should be `transform: translateX`); logo `image_tag` missing `fetchpriority: 'high'`; hardcoded `top: 68px` on `.gg-mega` is brittle if announcement bar added
- **Cleared concerns:** `position: fixed` on mega panel is valid (escapes `.shopify-section` wrapper correctly); `--ember-hover` is defined; `image.alt | escape` pre-assignment pattern is correct; all Liquid tag pairs balanced
