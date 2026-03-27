This Product Requirements Document (PRD) serves as the singular, granular blueprint for the **GarageGains** Shopify storefront. It is designed to be utilized by Claude Code to build a modern, high-performance eCommerce site that balances a gritty, industrial gym aesthetic with sleek, professional UI.

---

## 1. Project Vision & Positioning
* **Company Name:** GarageGains (Australia).
* **Core Phrase:** REDEFINING PERFORMANCE.
* **Mission:** Provide commercial-grade equipment without the commercial price tag, built for hard-training everyday Australians.
* **Aesthetic Direction:** "Raw Industrial Premium." High-contrast, dark-mode environment, deep shadows, and dramatic lighting (per `image_84b24d.jpg`).
* **Target Device Performance:** Flawless transition across Mobile, Tablet, and Desktop using a mobile-first responsive strategy.

---

## 2. Visual Identity & Design System

### 2.1 Colour Palette
* **Forge Black (#111111):** Primary background and heavy structural elements.
* **Iron (#1E1E1E):** Secondary background for cards and section layering.
* **Steel (#5C5C5C):** Subtle dividers, secondary UI, and "New" badges.
* **Ash (#E2DED8):** Metadata and subtext for readability.
* **Bone (#F5F3EF):** Primary text for high contrast on dark backgrounds.
* **Ember (#C94A1E):** Primary accent for CTAs, sale badges, and performance highlights.

### 2.2 Typography
* **Display & Headings:** *Barlow Condensed* (Extra Bold/Black). All-caps for headlines to provide a high-impact, industrial feel.
* **Body & UI:** *DM Sans*. Modern, clean, and highly legible for navigation and descriptions.

### 2.3 UI Motifs
* **The "Performance Skew":** Buttons and labels must utilize a 20-degree skewed parallelogram shape to imply speed and movement, following the **Transform Fitness** design.
* **Desaturated Imagery:** Photography should be gritty, desaturated, and high-contrast to match gym culture.

---

## 3. Homepage Architecture (In Order)

| Section | Detail & Functionality | Visual Reference |
| :--- | :--- | :--- |
| **1. Full-Bleed Hero** | Full-width photo, 40% dark gradient, left-aligned Barlow headline, and two skewed CTAs: **[SHOP NOW]** and **[EXPLORE GEAR]**. | `image_84b24d.jpg` vibe. |
| **2. Trust Bar** | High-contrast row with 4 minimalist icons: Free Delivery, 30-Day Returns, Lifetime Warranty, Secure Payment. | Clean, Steel-colored icons. |
| **3. Scrolling Ticker** | CSS-looping marquee (Ember background, Black text). Content: RACKS • BARBELLS • PLATES • BENCHES. | High-speed motion loop. |
| **4. Shop By Category** | 3-column grid (6 categories) with high-res athlete imagery and Ember-colored "BEST SELLER" badges. Emoji fallbacks included. | Transform Fitness grid. |
| **5. Featured Gear** | 4-up grid pulling from a Shopify collection. Clean, white-background product shots to pop against dark themes. | High-contrast product cards. |
| **6. Email CTA** | Iron background, large Ember headline: "10% OFF YOUR FIRST ORDER." Syncs with Shopify newsletter. | Transform CTA spacing. |
| **7. Trust Badges** | 4-up grid: "Built for Australians," "3mm Steel Gauge," "Direct to Gym," "Commercial Grade." | Industrial blueprint icons. |
| **8. Customer Reviews** | 3-column minimalist layout on Iron backgrounds. Star ratings in Ember. Editable via Theme Editor. | Sportix template style. |

---

## 4. Collection & Product Page Specifications

### 4.1 Collection Page (PLP)
* **Header:** Dynamic hero banner with collection title/description on an Iron background.
* **Interactivity:** Product count, "Sort By" dropdown, and top-bar filtering (Price, Weight, Steel Gauge).
* **Responsive Grid:** Fluid transition: 4 columns (Desktop) → 3 columns (Tablet) → 2 columns (Mobile).
* **Automated Badging:** SALE (Ember), NEW (Steel), or BEST SELLER (Ember) auto-applied via product tags.

### 4.2 Product Page (PDP)
* **Gallery:** Main high-res image with a vertical thumbnail switcher.
* **Information:** Breadcrumbs, star ratings, and price with specific "Sale Savings" callouts.
* **Purchase Area:** * **Variants:** Skewed parallelogram "pill" selectors for weights/colors.
    * **Action:** Ember **"ADD TO CART"** button with Ajax sidebar cart update.
    * **Wishlist:** Heart icon button for saved items.
* **Trust Meta:** 3 dedicated rows: e.g., "Ships from Melbourne," "Lifetime Warranty."
* **Tabs:** Professional tabbed interface for **Description**, **Tech Specs** (3mm steel, load ratings), and **Shipping**.
* **Upsell:** "Complete Your Setup" related products grid.

---

## 5. Responsive & Technical Standards

### 5.1 Multi-Screen Optimization
* **Breakpoints:** Mobile (up to 809px), Tablet (810px to 1439px), Desktop (1440px+).
* **Navigation:** Desktop uses horizontal links; Mobile/Tablet uses a slide-out hamburger menu optimized for thumb-reachability.
* **Typography:** CSS `clamp()` functions for headlines to ensure they stay bold on desktop without breaking layouts on small screens.

### 5.2 Technical Workflow
* **Stack:** Shopify Online Store 2.0 (Liquid), Tailwind CSS, GitHub.
* **Claude Code Instructions:** Claude should generate custom Liquid sections and CSS modules to achieve the specific "Performance Skew" and marquee animations.
* **Performance:** All assets must be served in WebP format; lazy loading enabled for all sections below the fold.

---

## 6. Supplier & Product Sourcing

### 6.1 Primary Supplier — Morgan Sports Australia
* **Supplier:** [Morgan Sports](https://www.morgansports.com.au/)
* **Model:** Australian B2B wholesale only. Morgan Sports does **not** sell direct-to-consumer — GarageGains operates as an authorised retail partner.
* **Established:** 1988. Australian owned and operated.
* **Known Commercial Clients:** Snap Fitness, F45, World Gym, UFC Gym — the same supply chain that powers national gym chains.

### 6.2 Product Categories Available from Supplier
The following Morgan Sports categories are in scope for the GarageGains catalogue:
* **Weightlifting:** Barbells, plates, racks, benches, platforms — core range.
* **Functional Fitness:** Strength training and conditioning equipment.
* **Flooring & Mats:** Rubber tiles, jigsaw mats — essential for home-gym setups.
* **Boxing & MMA:** Gloves, bags, protective gear — potential secondary category.
* **Apparel:** Training clothing and accessories — potential add-on category.

### 6.3 Brand Collections (Morgan Sports Lines)
Products available under the following Morgan Sports branded collections: Classic, Endurance, Alpha Series, Aventus, B2 Bomber, 1918 Heritage. Future opportunity to explore **custom/private-label** products branded under GarageGains.

### 6.4 Implications for Site Build
* **Product Data:** All Shopify product listings (titles, descriptions, specs, images) must be sourced and formatted from Morgan Sports wholesale catalogue data.
* **Tech Specs Tab (PDP):** Steel gauge, load ratings, and dimension data will be pulled directly from Morgan Sports product specs — ensure the PDP tab structure accommodates this.
* **Credibility Copy:** The Morgan Sports supply chain (F45, World Gym, Snap Fitness) is a powerful trust signal. Copy such as *"The same commercial-grade gear trusted by Australia's leading gym chains"* should be used in Trust Badge sections and product descriptions where applicable.
* **Inventory Management:** Shopify inventory levels should be managed with supplier lead times in mind. Avoid displaying "In Stock" for items that rely on Morgan Sports stock availability without a buffer system.

---