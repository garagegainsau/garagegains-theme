---
name: CSS Conventions
description: Established CSS patterns across all GarageGains sections — variables, BEM naming, animation utilities, skew motif
type: project
---

## CSS custom properties (referenced across sections)

All sections assume these are declared globally (in assets/garagegains.css or layout/theme.liquid):
- `--ember: #C94A1E`
- `--bone: #F5F3EF`
- `--ash: #E2DED8`
- `--steel: #5C5C5C`
- `--iron: #1E1E1E`
- `--forge-black: #111111`
- `--border: rgba(255,255,255,0.08)` — standard divider/border opacity

## BEM class naming pattern

All GarageGains sections use `gg-[section-name]__[element]--[modifier]`:
- Block: `.gg-purchase`, `.gg-gallery`, `.gg-pinfo`, `.gg-trust`, `.gg-tabs`, `.gg-upsell`
- Element: `.gg-purchase__qty-row`, `.gg-gallery__thumbs`, etc.
- Modifier: `.gg-variant-pill.is-selected`, `.gg-gallery__slide.is-active`

State classes use `is-*` prefix: `is-active`, `is-selected`, `is-unavailable`, `is-loading`, `is-wished`

## The Performance Skew

Applied inline in HTML (not via Tailwind utility):
```css
.gg-skew { display: inline-block; transform: skewX(-20deg); }
.gg-skew span { display: inline-block; transform: skewX(20deg); }
```
Used for: CTA buttons, variant pills, product badges, the ATC button in gg-upsell.

Skew badge variant (smaller, for overlays):
```css
.gg-skew-badge { transform: skewX(-20deg); background: var(--ember); padding: 4px 12px; }
.gg-skew-badge span { transform: skewX(20deg); }
```

## Shimmer/skeleton animation

Defined in gg-upsell.liquid — reuse pattern for any loading skeleton:
```css
background: linear-gradient(90deg, #1e1e1e 25%, #2a2a2a 50%, #1e1e1e 75%);
background-size: 200% 100%;
animation: gg-shimmer 1.4s infinite;
@keyframes gg-shimmer {
  0%   { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

## CSS tab switching (no JS)

Used in gg-product-tabs.liquid. Pattern:
- Hidden radio inputs rendered before the nav and panels
- Labels with `for=` matching input IDs serve as the clickable tabs
- `:checked ~ .gg-tabs__nav .gg-tabs__label[for="..."]` selector activates the correct label
- `:checked ~ .gg-tabs__panel--*` shows the correct panel
- All panels default to `display: none`; active panel set to `display: block`

**Why:** Zero JS dependency for tab UI. Works in Theme Editor preview. Accessible via keyboard
since radio inputs are focusable (though visually hidden).

## Section isolation

Every section scopes all its CSS to its own root class (`.gg-trust`, `.gg-tabs`, etc.)
and includes a `<style>` block within the section file. No shared stylesheets between
custom sections — each section is self-contained.
