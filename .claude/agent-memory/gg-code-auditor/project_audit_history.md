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

## feature/nav-v3 — 3-font system + announcement bar (layout/theme.liquid, assets/garagegains.css, sections/gg-header.liquid)
- **Audited:** 2026-04-26
- **Status:** FAIL (4 Critical, 5 Warnings)
- **Critical issues:**
  - C1: `gg-header.liquid` lines 122, 251, 267 still declare `font-family: 'DM Sans'` inside `<style>` block — font not loaded, will fall back to sans-serif
  - C2: `.gg-announce` is outside `<header>` but `.gg-nav` is `position: sticky` — announce bar will scroll away independently; mega menu `top: 72px` does not account for announce bar height (34px), so mega panel overlaps bar content when page is at top
  - C3: `garagegains.css` custom property `--iron` is `#0D0D0D` but CLAUDE.md spec is `#1E1E1E`; `--steel` is `#888888` vs spec `#5C5C5C` — colour system drift, pre-dates this branch but still blocking
  - C4: Schema has no `presets` block — header cannot be added via Theme Editor "Add section" flow (structural requirement for all GG sections)
- **Key warnings:** `.gg-announce__track` has no `width: max-content` or `min-width` — on narrow viewports the track may not be wide enough to fill, breaking seamless loop; `gg-review-card__stars` uses `#F5A623` not Ember `#C94A1E`; `--iron` value drift; `clip-path` pixel values (`7px`, `12px`) do not scale with element — use `%` or `calc()` for robustness; announcement bar has no reduced-motion media query for `announce-scroll`
- **Cleared concerns:** Liquid `split` / double-`for` loop for announce bar is correct; `announce-scroll` `-50%` keyframe is mathematically correct for doubled content; `transform: none !important` on `.gg-btn--header-cta` correctly overrides base `.gg-btn` skewX (higher specificity via extra class); `clip-path polygon` syntax is valid CSS; removing `border-color` from `.scrolled` is safe (border-color is now always set on `.gg-nav` base rule); all Liquid tag pairs balanced; schema `announcement_text` field is valid Shopify schema JSON

## feature/nav-v3 — second-pass audit (commit 041091d fixes verification)
- **Audited:** 2026-04-26
- **Status:** PASS WITH WARNINGS (0 Critical, 3 Warnings)
- **All 4 criticals and 5 warnings from first pass verified as fixed** except:
  - W3 (REMAINING): `.gg-nav__link` uses animated `padding-left` on hover via `transition` — not listed in the original fix batch but was flagged W3 in first audit; still present in commit 041091d; animates a layout property which violates performance rules
  - NEW W-A: `--ash` change from `rgba(255,255,255,0.6)` → `#E2DED8` is correct per spec but `var(--ash)` is only used in ONE place (`garagegains.css:236`, `.gg-nav__link` colour). No semi-transparent overlay usage of `--ash` found, so no rendering regressions.
  - NEW W-B: `gg-email-cta.liquid` success/error messages use inline `style` attributes with hardcoded `font-family:'Archivo Narrow'` rather than a CSS class — minor but inconsistent with the project's class-based styling approach.
  - NEW W-C: `gg-consultation.liquid` CTA button (`.gg-consult__btn`) uses its own `transform: skewX(-20deg)` pattern instead of the shared `.gg-btn.gg-btn--primary` system — CSS duplication, acceptable but creates drift risk.
- **Cleared concerns:** DM Sans completely purged from all .liquid and .css files (grep returned 0 results); `--iron`, `--steel`, `--ash` all correct per CLAUDE.md; `width: max-content` confirmed on `.gg-announce__track`; reduced-motion guard confirmed; star ratings now use `var(--ember)`; logo `fetchpriority: 'high'` confirmed; presets block confirmed in gg-header schema; mega `top` JS fix confirmed (`nav.getBoundingClientRect().bottom`).
