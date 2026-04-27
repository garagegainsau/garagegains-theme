# GarageGains — Shopify AR Feature Overview

**Feature Brief &nbsp;·&nbsp; Product Page Enhancement &nbsp;·&nbsp; April 2026**

---

## Overview

Augmented Reality (AR) can be added to GarageGains product pages, allowing customers to visualise gym equipment in their own home gym via smartphone — no app download required.

---

## 1. How Shopify AR Works

Shopify has built-in AR support via Google's `<model-viewer>` web component — already bundled inside the Dawn base theme GarageGains is built on. No third-party app or monthly subscription needed.

| Platform | Experience | File Required |
| :--- | :--- | :--- |
| **iOS (iPhone / iPad)** | Taps "View in AR" → Apple's native **Quick Look** AR viewer launches, placing the equipment in the room via camera | `.usdz` |
| **Android** | Opens **Google Scene Viewer** AR directly in the browser | `.glb` |
| **Desktop** | Interactive 3D rotate/zoom view (no AR, camera not available) | `.glb` |

---

## 2. What's Required

1. **3D model files for each product** — a `.glb` file (Android/desktop) and/or `.usdz` file (iOS). This is the main prerequisite — see Section 3 for how to source them.
2. **Upload to Shopify** — add the 3D file as product media (same workflow as uploading images). Shopify automatically detects the file type.
3. **Liquid code on the PDP** — the AR viewer panel is wired into `gg-product-main.liquid`. Claude Code handles all of this once a model file is available.

---

## 3. Sourcing 3D Model Files

| Method | How It Works | Estimated Cost | Quality |
| :--- | :--- | :--- | :--- |
| Commission a 3D artist | Provide product photos/dimensions; artist delivers `.glb` + `.usdz` | ~$50–$200 / product | Best — production-grade |
| Photogrammetry (Capture app / Matterport) | Photograph the real product from multiple angles; software auto-generates a 3D model | Free – $50 | Good for solid objects |
| AI-to-3D (Luma AI / Meshy.ai) | Upload photos; AI generates a 3D mesh automatically | Free tier available | Variable — test first |
| Morgan Sports catalogue | Ask the supplier directly — some wholesalers provide 3D assets for retail partners | Likely free | Official, accurate |

---

## 4. What Claude Code Will Build

Once a `.glb` file is available, Claude Code can implement:

- **AR viewer panel** embedded in the product page that activates automatically when a 3D model is attached to a product
- **Device detection** — correct AR button for iOS (Quick Look) vs Android (Scene Viewer) vs desktop (3D rotate-only)
- **Graceful fallback** — if no 3D model is uploaded, the section renders a standard product image with no broken UI
- **Branded AR button** using the GarageGains skewed parallelogram button aesthetic
- **No third-party dependencies** — uses Shopify's native `model-viewer` component only

---

## 5. The Key Constraint

> Claude Code can write all the Liquid, JavaScript, and CSS for the AR feature. The one thing it cannot do is generate 3D model files — those must be sourced externally. Once a single `.glb` file is ready, the complete AR feature can be built and tested live on the GarageGains product page immediately.

---

## Recommended Next Steps

1. **Contact Morgan Sports** — ask if 3D model assets (`.glb` / `.usdz`) are available in their wholesale catalogue. Fastest and most accurate path.
2. **Choose a sourcing method** — if Morgan Sports don't supply models, try Luma AI (free, test quality first) or commission a 3D artist for flagship products.
3. **Provide one `.glb` file to Claude Code** — the full AR viewer will be built and deployed to the GarageGains PDP immediately, ready for testing on a real device.

---

*GarageGains · Internal Feature Brief · Confidential*
