---
name: GarageGains z-index stacking context
description: Confirmed z-index values for header and overlay elements from garagegains.css
type: project
---

`.gg-nav` is `z-index: 200` (garagegains.css line 133). Any panel that must appear above the nav (mega menus, drawers) must use `z-index >= 201`. The mobile drawer panel is `z-index: 500` (garagegains.css line 370).

**Why:** gg-header.liquid was originally written with `.gg-mega { z-index: 199 }` — one below the nav — causing mega panels to render behind the sticky header bar. This was caught in the first audit of the rewritten header.

**How to apply:** Any new overlay, modal, or dropdown that must appear above the header bar must start at `z-index: 201` or higher. Never assume a stacking value without cross-referencing garagegains.css `:root` or the `.gg-nav` rule.
