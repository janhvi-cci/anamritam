# ANAMRITAM — UI/UX Redesign & Modernization Plan
**Design Reference:** [Ayurique](https://ayurique.vercel.app/)  
**Target Codebase:** [Anamritam](https://github.com/janhvi-cci/anamritam)  
**Target URL:** [Anamritam Live](https://anamritam.vercel.app/)  
**Status:** Planning & Analysis Phase (Zero source code modifications made)

---

## 1. PROJECT OVERVIEW

### 1.1 Current Architecture & Tech Stack
Anamritam is a modern, lightweight e-commerce web application built on:
- **Framework:** React 18.3 (`react`, `react-dom`) with TypeScript (`5.5.3`)
- **Build Tool / Bundler:** Vite 5.4 with `@vitejs/plugin-react`
- **Styling:** Tailwind CSS 3.4 + Custom CSS architecture in `src/index.css`
- **Icons:** `lucide-react` (0.446.0)
- **External Integration:** Google Forms for order submissions via prefilled entry IDs (`src/config.ts`)
- **State Management:** React local state (`useState`, `useMemo`, `useEffect`) with `localStorage` persistence
- **Routing:** SPA single-page tab/state-driven navigation (`'home' | 'products' | 'story' | 'contact' | 'cart' | 'login' | 'register' | 'checkout' | 'success' | 'product'`)

### 1.2 Codebase Structure
```
anamritam/
├── public/
│   └── images/
│       ├── anamritum_logo.jpg
│       └── products/
│           ├── mahua_ladoo.jpg
│           ├── mahua_ladoo_hover1.jpg
│           └── mahua_ladoo_hover2.jpg
├── src/
│   ├── App.tsx          # Single-file monolith containing all page views, components, state, and handlers
│   ├── config.ts       # Google Form IDs, URLs, business contact information
│   ├── index.css       # Global design variables, layout classes, responsive breakpoints
│   ├── main.tsx        # React entrypoint
│   └── vite-env.d.ts   # Vite typing
├── index.html          # Metadata, font links (DM Sans, Playfair Display)
├── tailwind.config.js  # Base Tailwind configuration
├── package.json
└── tsconfig.app.json
```

---

## 2. EXISTING FUNCTIONALITY (PROTECTED LOGIC)

The website contains robust, fully operational business logic that must **NOT** be broken, rebuilt, or rewritten:

1. **State-Driven Single-Page Routing:**
   - Seamless switching between `'home'`, `'products'`, `'story'`, `'contact'`, `'cart'`, `'login'`, `'register'`, `'checkout'`, `'success'`, and `'product'` detail views with automatic top-scroll behavior.
2. **Product Catalog & Categorization:**
   - 7 defined products across `'Ladoos'` and `'Protein Bars'` categories.
   - Category filtering (`'All' | 'Ladoos' | 'Protein Bars'`) and sorting (`'Featured' | 'Price: Low to High' | 'Price: High to Low'`).
3. **Cart Operations & Drawer:**
   - Add to cart with custom quantity.
   - Persistent cart state in `localStorage` under `'anamritam-cart'`.
   - Live badge quantity counter in navbar.
   - Slide-out Cart Drawer with backdrop overlay and click-outside dismissal.
   - Dedicated Cart Page view.
   - Quantity increments/decrements (clamped $\ge 1$) and item removal.
   - Automatic subtotal calculations with Indian Rupee formatting (`₹` / `toLocaleString('en-IN')`).
4. **User Authentication & Session Handling:**
   - Register form with validation (email regex, 10-digit mobile number, matching passwords).
   - Login with session persistence in `localStorage` under `'anamritam-session'` and `'anamritam-account'`.
   - Navbar user display (first name derivation) and sign-out handler.
5. **Checkout & Google Forms Integration:**
   - 2-step checkout flow (Details Entry $\to$ Order Review $\to$ Google Form Submission).
   - Order ID generation algorithm (`ANM-YYYYMMDD-XXXX`).
   - Order details string serialization (`quantity × product.name`).
   - Exact query parameter encoding mapping form fields to `GOOGLE_FORM_FIELD_IDS` in `src/config.ts`.
   - Redirection to `GOOGLE_FORM_URL` with prefilled values.
6. **External Links & Contact Info:**
   - Instagram deep link.
   - Direct `mailto:` and `tel:` links bound to `CONTACT_DETAILS` in `src/config.ts`.

---

## 3. CURRENT UI/UX ANALYSIS (ANAMRITAM)

### What Anamritam Currently Does Well
- **Warm Brand Foundation:** Earthy color palette (`#164a2b` deep forest green, `#f7f3e8` cream background, `#d69a2d` gold accent) that fits Ayurvedic/traditional Indian nourishment.
- **Clear Information Architecture:** Straightforward navigation structure; pages are easy to discover.
- **Fast Performance:** Single-bundle SPA with zero heavy dependencies; instant page transitions and minimal load times.
- **Complete User Flow:** End-to-end purchasing journey from product discovery $\to$ cart drawer $\to$ checkout review $\to$ Google Form dispatch.

### Areas Needing Visual & UX Polish
1. **Typography Hierarchy & Balance:**
   - Heading sizes can feel disproportionate or abrupt across viewport widths.
   - Spacing between eyebrows, headings, subtitles, and CTAs lacks rhythmic vertical scale.
2. **Navigation Bar & Header:**
   - Header is visually flat; lacks subtle blur transitions, active link indicator states, and interactive feedback.
   - Mobile menu is a simple block drop-down without smooth opening animations, backdrop blur, or luxury styling.
   - No dark/light theme switch or language translation controls.
3. **Hero Section:**
   - Hero visual layout lacks depth and layered framing.
   - Badge is positioned statically with a plain circle; missing layered gold hairline borders and editorial stamp flair.
4. **Product Presentation & Cards:**
   - Product cards are static and rectangular; product images do not have secondary image hover swap (despite hover assets existing in `public/images/products/`).
   - Add to cart button and quantity picker are cramped inside the small card footer on mobile.
   - Missing quick-view drawer/modal for fast previewing without navigating away from the catalog.
5. **Dark Mode & Theming:**
   - Currently hardcoded to light cream (`#f7f3e8`) without CSS variable tokenization for dark/light mode switching.
6. **Microinteractions & Transitions:**
   - Missing scroll-triggered reveal animations (`reveal` on scroll).
   - Buttons have basic `translateY` hover but lack refined luxury elevation, subtle shimmer, or smooth active states.
7. **Cart Drawer & Checkout Polish:**
   - Cart Drawer and Checkout views are clean but lack editorial borders, coupon code inputs, subtle empty state illustrations, and tactile feedback.

---

## 4. AYURIQUE REFERENCE ANALYSIS

Inspecting [Ayurique](https://ayurique.vercel.app/) reveals key design principles that give it an ultra-premium, luxury Ayurvedic apothecary aesthetic:

1. **Editorial Typography System:**
   - Pairings of `DM Sans` (for clean, legible body copy, numbers, and UI labels) with `Playfair Display` (for elegant titles, italicized accents, and display quotes).
   - Consistent use of tracked uppercase eyebrows (`font-size: 10px; letter-spacing: 0.22em; font-weight: 600; text-transform: uppercase;`).
   - Dynamic italic emphasis using `em` tags highlighted in soft gold (`--gold: #b89a5a`).
2. **Layered Earth & Botanical Color Hierarchy:**
   - Backgrounds built with stepped organic layers: Ivory (`#f8f5ed`), Cream (`#f2ecdf`), and Sand (`#e8e0cf`).
   - Deep Botanical Forest (`#173a32`) for hero CTAs, category card blocks, and footer.
   - Rich Ayurvedic Gold (`#b89a5a` / `#d8c89e`) for accents, badges, hairline borders, and hover underlines.
   - Warm Ink (`#3c3028`) for typography instead of pure black `#000000`, providing softness.
3. **Atmospheric Visuals & Layered Frames:**
   - Images placed inside double-framed offset borders (`.hero-frame:before` offset border).
   - Floating editorial stamps (`.hero-stamp`) with gold typography on dark green backings.
   - Subtle background radial glow effects (`.hero-glow`).
4. **Interactive Product Hover Dynamics:**
   - Dual image hover swap: primary image smooth-fades to secondary detail image with subtle scale transition (`transform: scale(1.03); opacity: 0 -> 1`).
   - Floating "Quick Add" / "Quick View" action bar that slides up from the card bottom on hover (`translateY(8px) -> translateY(0); opacity: 0 -> 1`).
   - Wishlist button at the top-right.
5. **Interactive Navigation & Slide-Out Panels:**
   - Sticky navbar with blur backdrop (`backdrop-filter: blur(18px)`).
   - Hover underline animation that expands from 0% to 100% width on nav links.
   - Rotating ticker announcement bar with auto-cycling highlights.
   - Slide-out Cart Drawer with smooth spring easing and clean item lists.
6. **Consistent Microinteractions:**
   - Viewport scroll reveal (`.reveal.is-visible`).
   - Pill and chip hover interactions with gold border accents.
   - Accessible motion suppression via `@media (prefers-reduced-motion: reduce)`.

---

## 5. COMPARISON: ANAMRITAM vs. AYURIQUE

| Aspect | Anamritam (Current State) | Ayurique (Design Reference) | Redesign Adaptation for Anamritam |
| :--- | :--- | :--- | :--- |
| **Brand Identity** | Wholesome traditional Indian food (Mahua Ladoos, Protein Bars) | Luxury Ayurvedic personal care rituals | **Preserve 100% of Anamritam food/wellness branding, logo, and product identity** while borrowing Ayurique’s layout polish |
| **Color System** | Basic green (`#164a2b`), cream (`#f7f3e8`), gold (`#d69a2d`) | Refined multi-tier palette (Ivory, Cream, Sand, Forest, Botanical, Gold, Ink) | Expand Anamritam’s colors into semantic CSS custom properties supporting **Light & Dark modes** |
| **Navbar** | Static cream navbar, standard text buttons | Frosted blur navbar, animated gold underline on hover, icon actions | Upgrade to frosted blur sticky navbar with active indicators, theme toggle, and translate control |
| **Mobile Menu** | Simple dropdown below header | Full-screen editorial overlay with Playfair headings and footer info | Add animated full-screen/slide mobile drawer with elegant typography |
| **Hero Section** | Standard 2-column grid with simple image | Offset double-framed image, radial background glow, editorial badge stamp | Integrate Ayurique’s offset frame, radial warm glow, and floating badge |
| **Product Cards** | Single static image, standard add button | Dual-image hover swap, slide-up quick action, refined typography | Add dual-image hover (utilizing existing hover assets), slide-up quick add, and gold tag badges |
| **Theme Toggle** | Not implemented | Light mode only | **Add comprehensive Dark/Light theme** with `localStorage` persistence |
| **Google Translate**| Not implemented | Not present | **Add clean Google Translate dropdown** seamlessly integrated into nav |
| **Animations** | Basic CSS keyframes for drawer and toast | Smooth scroll reveals (`reveal`), button elevation, hover zooms | Implement lightweight scroll reveals and microinteractions with `prefers-reduced-motion` |
| **Cart & Checkout**| Functional drawer, page, and review step | Polished modal/drawer with coupon inputs and order breakdown | **Restyle existing working views** into high-end layout without changing logic |
| **Order Dispatch** | Google Forms prefilled redirect | Simulated checkout | **Preserve exact Google Forms prefill integration** |

---

## 6. RECOMMENDED DESIGN SYSTEM

### 6.1 Color Architecture (CSS Variables / Tokens)
We will define semantic CSS custom properties on `:root` and `[data-theme="dark"]` / `.dark` so that all components adapt automatically.

```css
/* Light Theme (Default) */
:root {
  --bg-primary: #f8f5ed;       /* Ivory */
  --bg-secondary: #f2ecdf;     /* Soft Cream */
  --bg-tertiary: #e8e0cf;      /* Warm Sand */
  --bg-card: #fcfbfa;          /* Crisp Card Base */
  --bg-overlay: rgba(23, 58, 50, 0.45);
  
  --text-primary: #2b231d;     /* Deep Warm Ink */
  --text-secondary: #5f5449;   /* Muted Ink */
  --text-tertiary: #8a7e71;    /* Subtle Caption */
  
  --brand-forest: #143d2b;     /* Anamritam Signature Deep Green */
  --brand-botanical: #1d523b;  /* Botanical Medium Green */
  --brand-sage: #2f6b3c;       /* Herbaceous Accent */
  --brand-gold: #c2933a;       /* Ayurvedic Warm Gold */
  --brand-gold-soft: #dec89c;  /* Pale Gold */
  --brand-warm-terracotta: #b86248; /* Accent Warmth / Delete / Error */
  
  --border-hairline: rgba(43, 35, 29, 0.1);
  --border-strong: rgba(43, 35, 29, 0.22);
  --border-gold: rgba(194, 147, 58, 0.4);
  
  --shadow-subtle: 0 2px 10px rgba(20, 61, 43, 0.05);
  --shadow-card: 0 8px 24px rgba(20, 61, 43, 0.08);
  --shadow-elevation: 0 16px 40px rgba(20, 61, 43, 0.12);
  
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 14px;
  --radius-full: 9999px;
  
  --font-sans: 'DM Sans', system-ui, -apple-system, sans-serif;
  --font-serif: 'Playfair Display', Georgia, serif;
}

/* Dark Theme */
[data-theme="dark"] {
  --bg-primary: #0f1c16;       /* Deep Botanical Forest Midnight */
  --bg-secondary: #14241d;     /* Deep Botanical Green-Black */
  --bg-tertiary: #1b2e25;      /* Elevated Dark Surface */
  --bg-card: #182821;          /* Card Background */
  --bg-overlay: rgba(0, 0, 0, 0.7);
  
  --text-primary: #f5f0e6;     /* Pale Cream */
  --text-secondary: #c0b9ad;   /* Soft Ivory */
  --text-tertiary: #8a887e;    /* Muted Dark */
  
  --brand-forest: #22573f;     /* Elevated Green */
  --brand-botanical: #2f7053;
  --brand-sage: #4f8c61;
  --brand-gold: #d4a754;       /* Radiant Warm Gold */
  --brand-gold-soft: #f0debb;
  --brand-warm-terracotta: #e07a60;
  
  --border-hairline: rgba(245, 240, 230, 0.1);
  --border-strong: rgba(245, 240, 230, 0.2);
  --border-gold: rgba(212, 167, 84, 0.4);
  
  --shadow-subtle: 0 2px 10px rgba(0, 0, 0, 0.25);
  --shadow-card: 0 8px 24px rgba(0, 0, 0, 0.35);
  --shadow-elevation: 0 16px 40px rgba(0, 0, 0, 0.45);
}
```

### 6.2 Typography Scale & Rules
- **Eyebrow:** `font-family: var(--font-sans); font-size: 10px; font-weight: 600; letter-spacing: 0.22em; text-transform: uppercase; color: var(--brand-sage);`
- **Hero Title:** `font-family: var(--font-serif); font-size: clamp(2.75rem, 5.5vw, 5rem); line-height: 1.08; font-weight: 500;`
- **Hero Title `em`:** `font-family: var(--font-serif); font-style: italic; color: var(--brand-gold);`
- **Section Heading:** `font-family: var(--font-serif); font-size: clamp(2rem, 3.8vw, 3.5rem); line-height: 1.15;`
- **Product Title:** `font-family: var(--font-serif); font-size: 1.25rem; font-weight: 500; color: var(--text-primary);`
- **Body Copy:** `font-family: var(--font-sans); font-size: 0.95rem - 1.05rem; line-height: 1.7; color: var(--text-secondary);`
- **Price:** `font-family: var(--font-serif); font-size: 1.15rem; font-weight: 600; color: var(--text-primary);`
- **Buttons / Actions:** `font-family: var(--font-sans); font-size: 11px; font-weight: 600; letter-spacing: 0.16em; text-transform: uppercase;`

### 6.3 Buttons & Interactive Controls
- **Primary Button (`.button-dark`):** Solid `--brand-forest`, text `#ffffff`, border-radius 6px, uppercase tracked text, elevation on hover (`transform: translateY(-2px); box-shadow: var(--shadow-card)`).
- **Secondary Button (`.button-outline`):** Transparent background, 1px border `--border-strong`, hover transitions border to `--brand-gold` and text to `--text-primary`.
- **Editorial Link Button (`.link-button` / `.underlined-link`):** Underlined with `--brand-gold`, arrow icon with hover translate animation (`gap: 8px -> 14px`).
- **Quantity Selector:** Crisp border, neutral background, icon buttons with smooth hover feedback.

### 6.4 Card System
- **Product Cards:** Clean rectangular cards with subtle border or shadow, aspect ratio `.85`, overflow-hidden image container with dual-image crossfade hover effect.
- **Category Cards:** Large cards with background image zoom on hover, dark linear-gradient overlay, Playfair typography, and gold accent numbers (`01`, `02`).
- **Why Anamritam Grid:** 4-column cards with top hairline gold/green accent border, Lucide icons in gold circles, bold title, and refined description.

---

## 7. COMPONENT PLAN

To adhere to the core principle of **Restyle over Rebuild** and **Reuse over Rewrite**, the component breakdown is categorized below:

### 7.1 Components to RESTYLE (Keep 100% existing logic, upgrade markup & CSS)
1. `Announcement`: Upgrade to rotating/cycling announcement bar with auto-interval track.
2. `Navbar`: Add sticky frosted glass blur, animated gold underline on nav buttons, theme toggle button, translation control, polished cart counter badge.
3. `MobileMenu`: Upgrade from basic block to animated editorial overlay with `Playfair Display` links and contact footer.
4. `Home` Hero Section: Add background radial glow, dual-border offset frame (`.hero-frame`), and gold-trimmed circular stamp badge.
5. `CategorySection`: Refine cards with smooth image scale, gold numbers (`01`, `02`), and luxury typography.
6. `ProductCard`: Integrate dual-image hover swap (using `mahua_ladoo.jpg` & `mahua_ladoo_hover1.jpg`/`hover2.jpg`), slide-up quick add trigger, and gold tag badge.
7. `WhySection`: Restyle into 4-column balanced grid with gold-accented icons and clean typography.
8. `StoryBanner` & `InstagramSection`: Refine 6-tile grid with zoom-on-hover and dark overlay.
9. `Products` (Catalog Page): Upgrade filter tabs with animated underline active states, custom styled sort dropdown, and refined grid layout.
10. `ProductDetails` (Detail View): Add thumbnail gallery row if multiple images exist, tab switcher with active gold indicator, clean metadata table, and clear Add to Cart CTA.
11. `CartDrawer` & `CartPage`: Restyle item rows, mini-quantity selectors, trash icons, subtotal summary card, and continue shopping actions.
12. `Register`, `Login`, `AccountShell`: Restyle split-screen layout with warm background image, refined input fields with gold focus rings, and clear error banners.
13. `Checkout`, `OrderSummary`, `ReviewOrder`: Modernize 2-step layout, clean field grouping, and review summary box.
14. `Success`: High-end order success badge with animated checkmark and clear next-steps copy.
15. `Story` & `Contact`: Editorial multi-column layout with gold details.
16. `Footer`: 4-column luxury layout with brand lockup, social links, contact info, and legal line.

### 7.2 Components to REFACTOR (Internal structure improvements without changing props or logic)
1. `App.tsx` $\to$ Modular directory structure (in execution phase) or structured sub-components while keeping `App.tsx` state and handlers intact.
2. Form field inputs $\to$ Refactor `FormField` to support crisp floating/top labels with standard accessibility IDs.

### 7.3 Components to REUSE (Directly preserved without structural change)
1. `readStorage`, `money`, `generateOrderId` helper functions.
2. `products` data array in `src/App.tsx`.
3. `GOOGLE_FORM_FIELD_IDS`, `GOOGLE_FORM_URL`, `CONTACT_DETAILS` in `src/config.ts`.
4. All `lucide-react` icon imports.

### 7.4 Newly Created UI Sub-Components (Small, lightweight helpers)
1. `ThemeToggle`: Sun/Moon toggle button with smooth icon rotation, interacting with theme state and `localStorage`.
2. `GoogleTranslate`: Embedded lightweight Google Translate dropdown with custom aesthetic matching Anamritam.
3. `QuickViewModal` (Optional Phase 5 addition): Modal preview for fast product browsing without full page navigation.

---

## 8. PHASE-BY-PHASE IMPLEMENTATION PLAN

```
┌─────────────────────────────────────────────────────────────┐
│                       PHASE ROADMAP                         │
├─────────────┬───────────────────────────────────────────────┤
│ Phase 1     │ Dark / Light Theme System (CSS Variables)     │
│ Phase 2     │ Navbar & Mobile Navigation Modernization      │
│ Phase 3     │ Google Translate Integration                  │
│ Phase 4     │ Overall UI/UX Redesign (Hero, Sections, Home) │
│ Phase 5     │ Product Catalog, Cards & Detail UI Polish     │
│ Phase 6     │ Microinteractions, Animations & Motion QA     │
│ Phase 7     │ Cart, Checkout & Account Polish               │
│ Phase 8     │ Final QA, Responsiveness & Verification       │
└─────────────┴───────────────────────────────────────────────┘
```

---

### PHASE 1 — DARK / LIGHT THEME SYSTEM
- **Goal:** Provide a seamless, persistent Dark/Light theme across the entire application with Light theme as the default.
- **Changes:**
  - Define semantic theme tokens in `src/index.css` under `:root` and `[data-theme="dark"]`.
  - Add theme state handler with `localStorage` key `'anamritam-theme'`.
  - Ensure system preference detection (`prefers-color-scheme`) with manual toggle override.
  - Implement smooth CSS color transitions (`transition: background-color 0.3s ease, color 0.3s ease, border-color 0.3s ease`).
  - Maintain image clarity and contrast in dark mode (prevent unwanted filter inversions).
- **Files Likely Affected:**
  - `src/index.css`
  - `src/App.tsx` (or new `ThemeContext`/`ThemeToggle`)
  - `index.html` (initial theme script to prevent flash of wrong theme)
- **Protected Functionality:** All existing layout dimensions and text content.
- **Risk Level:** Low.
- **Testing Required:** Toggle theme on all pages; verify persistence after reload; check contrast ratios in both modes.

---

### PHASE 2 — NAVBAR + MOBILE NAVBAR
- **Goal:** Create an ultra-clean, sticky navigation bar with frosted glass blur, animated hover states, and an editorial mobile overlay.
- **Changes:**
  - Add sticky glassmorphism style (`backdrop-filter: blur(18px); background: var(--bg-primary-translucent)`).
  - Add active state indicators and animated gold underlines on hover for desktop links.
  - Integrate `ThemeToggle` button and `GoogleTranslate` selector into navigation actions.
  - Add badge animation for cart item count.
  - Restructure mobile drawer: full-screen overlay with `Playfair Display` links, close animation, and direct access to Account/Cart.
- **Files Likely Affected:**
  - `src/App.tsx` (`Navbar`, `MobileMenu`)
  - `src/index.css`
- **Protected Functionality:**
  - Navigation handlers (`onNavigate`), cart drawer opener (`onCart`), mobile menu toggler (`onMenu`), logout action (`onLogout`).
- **Risk Level:** Low-Medium.
- **Testing Required:** Test desktop sticky scroll, mobile hamburger opening/closing, resize transitions, tap target sizes.

---

### PHASE 3 — GOOGLE TRANSLATE
- **Goal:** Embed actual, functional Google Translation into the navigation bar with a custom-styled, elegant control.
- **Changes:**
  - Inject Google Translate script in a clean, non-blocking manner.
  - Style the dropdown element to match the website's typography and color scheme (hiding default Google branding banners that distort layout).
  - Support translation across desktop and mobile navigation in both Light and Dark themes.
- **Files Likely Affected:**
  - `index.html`
  - `src/App.tsx` (`Navbar`, `MobileMenu`, new `GoogleTranslate` component)
  - `src/index.css`
- **Protected Functionality:** Existing page text and routing.
- **Risk Level:** Medium (must ensure Google Translate script does not inject disruptive CSS styles or break React DOM reconciliations).
- **Testing Required:** Select languages (Hindi, Spanish, French, etc.); verify dynamic text updates; ensure layout stays intact and theme toggle works simultaneously.

---

### PHASE 4 — OVERALL UI/UX REDESIGN (HOMEPAGE & SECTIONS)
- **Goal:** Elevate Homepage, Hero, About/Story, Category, and Instagram sections to Ayurique-inspired editorial quality while keeping Anamritam branding.
- **Changes:**
  - **Hero Section:** Add subtle radial warm glow, offset double-border image frame, Playfair Display heading with italic gold accent word, and circular brand stamp.
  - **Announcement Bar:** Rotating message ticker with smooth slide transitions.
  - **Intro / Philosophy Section:** Refined brand monogram ("A"), elegant typography, and gold quote divider.
  - **Category Cards:** Large cards with smooth image scale, dark gradient overlay, and gold numbering.
  - **Why Anamritam Section:** 4-column balanced grid with gold-accented icons and clean typography.
  - **Story Banner & Instagram Feed:** High-resolution image grid with hover zooms, caption overlays, and direct Instagram link.
  - **Footer:** 4-column luxury layout with newsletter snippet, social badges, contact info, and legal line.
- **Files Likely Affected:**
  - `src/App.tsx` (`Home`, `Hero`, `CategorySection`, `WhySection`, `InstagramSection`, `Footer`, `Story`, `Contact`)
  - `src/index.css`
- **Protected Functionality:** External links (Instagram), Contact emails/phones, navigation triggers.
- **Risk Level:** Low.
- **Testing Required:** Cross-device visual verification at 320px, 375px, 768px, 1024px, 1440px.

---

### PHASE 5 — PRODUCT UI (CARDS, CATALOG & DETAIL PAGE)
- **Goal:** Modernize product presentation, card hover behavior, catalog filtering, and product detail views.
- **Changes:**
  - **Product Card:**
    - Dual-image hover swap: primary image crossfades smoothly to hover image (`mahua_ladoo_hover1.jpg`/`hover2.jpg`) on hover.
    - Slide-up "Add to Cart" / "Quick View" action bar.
    - Category tag badge and clean price typography.
  - **Products Catalog Page:**
    - Clean filter tabs with animated active underline.
    - Custom styled sorting selector (Price Low-to-High, High-to-Low, Featured).
    - Grid layout adapting from 1 column (mobile) to 2 columns (tablet) to 4 columns (desktop).
  - **Product Detail Page:**
    - Large high-resolution gallery view.
    - Rating stars with subtitle badge.
    - Tabbed accordion/pills for Description, Ingredients, and Nutrition information.
    - Quantity selector and prominent Add to Cart button with stock note.
- **Files Likely Affected:**
  - `src/App.tsx` (`ProductCard`, `Products`, `ProductDetails`)
  - `src/index.css`
- **Protected Functionality:**
  - `products` data structure, `addToCart`, `openProduct`, category filtering, sorting logic.
- **Risk Level:** Low-Medium.
- **Testing Required:** Test quantity increments, image hover transitions, category filter switching, sorting accuracy, Add to Cart feedback.

---

### PHASE 6 — MICROINTERACTIONS & ANIMATIONS
- **Goal:** Implement subtle, high-end motion and interactive feedback across all UI components.
- **Changes:**
  - Scroll-triggered reveal transitions (`reveal` class with IntersectionObserver).
  - Button elevation and ripple/shimmer on hover and active click.
  - Navigation link underline animation (`:after` width expand).
  - Cart drawer slide-in animation with smooth backdrop fade.
  - Toast notification slide-and-fade animation.
  - Full support for `@media (prefers-reduced-motion: reduce)` to disable animations for accessibility.
- **Files Likely Affected:**
  - `src/index.css`
  - `src/App.tsx` (scroll observer hook)
- **Protected Functionality:** Component state and rendering performance.
- **Risk Level:** Low.
- **Testing Required:** Test with normal motion and with reduced motion enabled in OS/browser. Verify 60fps performance without layout jank.

---

### PHASE 7 — CART + CHECKOUT + ACCOUNT VISUAL POLISH
- **Goal:** Polish the visual layout of Cart Drawer, Cart Page, Checkout form, Review step, and Login/Register pages without altering business logic.
- **Changes:**
  - **Cart Drawer & Cart Page:**
    - Clean product thumbnail, title, unit price, quantity controls, and delete icon.
    - Order summary with subtotal, shipping note, and checkout CTA button.
  - **Checkout Page:**
    - 2-step visual progress indicator (`01 Customer Details` $\to$ `02 Order Review`).
    - Input fields with clean border lines, gold focus rings, and clear error text.
    - Review step with customer address summary and order summary card.
  - **Account Pages (Login & Register):**
    - Split-panel layout with atmospheric image on desktop.
    - Refined form fields and toggle links.
- **Files Likely Affected:**
  - `src/App.tsx` (`CartDrawer`, `CartItem`, `CartPage`, `Checkout`, `ReviewOrder`, `OrderSummary`, `Login`, `Register`, `Success`)
  - `src/index.css`
- **Protected Functionality:**
  - **CRITICAL:** `localStorage` cart serialization (`'anamritam-cart'`), quantity adjustments, item removals, `generateOrderId()`, `GOOGLE_FORM_FIELD_IDS` mapping, and `GOOGLE_FORM_URL` redirect with prefilled queries.
- **Risk Level:** Medium (must ensure form field names and values continue to match Google Form field IDs).
- **Testing Required:** Perform full checkout flow with test inputs; verify generated URL contains all Google Form field IDs; verify login/register validation and session persistence.

---

### PHASE 8 — FINAL QA, CROSS-BROWSER & MULTI-DEVICE VERIFICATION
- **Goal:** Perform rigorous end-to-end verification across devices, themes, and user journeys.
- **Testing Areas:**
  1. Desktop (Chrome, Safari, Edge, Firefox at 1920px, 1440px, 1280px).
  2. Tablet (iPad portrait & landscape at 768px - 1024px).
  3. Mobile (iPhone SE, iPhone 14/15, Android at 320px, 375px, 414px, 430px).
  4. Light Theme & Dark Theme visual consistency and contrast ratios.
  5. Google Translate functionality and layout stability.
  6. Add to cart, quantity update, cart drawer, cart page persistence.
  7. Checkout review and Google Form submission URL generation.
  8. Login, registration, session persistence, and sign-out.
  9. TypeScript compilation (`npm run typecheck` / `tsc --noEmit`).
  10. Production build verification (`npm run build`).

---

## 9. FILE IMPACT MAP

| Phase | Files Likely Affected | Risk Level | Reason & Scope |
| :--- | :--- | :---: | :--- |
| **Phase 1: Theming** | `src/index.css`<br>`src/App.tsx`<br>`index.html` | **Low** | Introduce CSS variables for Light/Dark tokens; add theme toggle button and persistence logic. |
| **Phase 2: Navigation** | `src/App.tsx`<br>`src/index.css` | **Low-Medium** | Modernize `Navbar` and `MobileMenu`; add sticky blur, active indicators, and menu animations. |
| **Phase 3: Translation** | `index.html`<br>`src/App.tsx`<br>`src/index.css` | **Medium** | Embed Google Translate script and custom-styled dropdown without breaking existing styles. |
| **Phase 4: UI/UX Redesign** | `src/App.tsx`<br>`src/index.css` | **Low** | Restyle Homepage, Hero, Categories, Why Anamritam, Instagram, and Footer sections. |
| **Phase 5: Product UI** | `src/App.tsx`<br>`src/index.css` | **Low-Medium** | Upgrade `ProductCard` (dual-image hover), `Products` catalog, and `ProductDetails` views. |
| **Phase 6: Microinteractions** | `src/index.css`<br>`src/App.tsx` | **Low** | Add scroll reveals (`reveal`), button elevation, hover effects, and reduced-motion support. |
| **Phase 7: Cart & Checkout** | `src/App.tsx`<br>`src/index.css` | **Medium** | Visual polish for Cart Drawer, Cart Page, Checkout 2-step review, and Google Form redirect. |
| **Phase 8: Final QA** | All files (verification only) | **Low** | Run builds, type checks, lint checks, and multi-device manual testing. |

---

## 10. PROTECTED FUNCTIONALITY CHECKLIST

> [!IMPORTANT]
> The following list represents core application integrity. During subsequent implementation phases, none of these items may be removed, broken, or rewritten:

- [x] **Cart State & Persistence:** `localStorage.getItem('anamritam-cart')` and `localStorage.setItem('anamritam-cart')`
- [x] **Cart Calculations:** Subtotal math `line.product.price * line.quantity` and Indian Rupee formatting (`money()`)
- [x] **Cart Modifications:** `addToCart()`, `changeQuantity()`, `removeItem()`
- [x] **Single-Page Navigation:** State-based `page` transitions (`'home' | 'products' | 'story' | 'contact' | 'cart' | 'login' | 'register' | 'checkout' | 'success' | 'product'`)
- [x] **Product Data Integrity:** All 7 products (`plain-elaichi`, `elaichi-ginger`, `masala-mahua`, `mirch-mahua`, `plain-protein`, `mahua-protein`, `sesame-crunch`) with their exact IDs, prices, categories, and descriptions
- [x] **Google Form Field Mapping:**
  - `name`: `entry.2005620554`
  - `email`: `entry.1045781291`
  - `address`: `entry.1065046570`
  - `phone`: `entry.1166974658`
  - `city`: `entry.839337160`
  - `state`: `entry.1538695280`
  - `pincode`: `entry.544210790`
  - `orderDetails`: `entry.1351395543`
  - `quantity`: `entry.349550832`
  - `total`: `entry.656264044`
  - `orderId`: `entry.993607380`
- [x] **Order ID Format:** `generateOrderId()` generating `ANM-YYYYMMDD-XXXX`
- [x] **External Business Data:** `CONTACT_DETAILS` (email, phone, address) and Instagram link in `src/config.ts`
- [x] **User Authentication Persistence:** `localStorage.getItem('anamritam-session')` and `localStorage.getItem('anamritam-account')`

---

## 11. TESTING CHECKLIST

### Desktop Testing ($\ge 1024\text{px}$)
- [ ] Frosted glass sticky navbar appears on scroll.
- [ ] Nav links display animated gold underline on hover.
- [ ] Theme toggle switches between Light and Dark mode smoothly.
- [ ] Google Translate dropdown translates content without breaking layout.
- [ ] Hero section displays offset framed visual and radial glow.
- [ ] Category cards scale images smoothly on hover.
- [ ] Product cards perform dual-image crossfade on hover.
- [ ] Add to cart opens drawer / triggers toast notification.
- [ ] Cart drawer subtotal and quantity controls function properly.
- [ ] Checkout 2-step review displays accurate customer details.
- [ ] Google Form redirect opens with all prefilled parameters.

### Tablet Testing ($768\text{px} - 1023\text{px}$)
- [ ] Navigation collapses cleanly without overlapping elements.
- [ ] Hero grid adapts to stacked / tablet proportions.
- [ ] Product catalog displays in 2-column or 3-column grid.
- [ ] Cart drawer occupies appropriate width (max 440px).
- [ ] Checkout layout stacks vertically with order summary at the top or bottom.

### Mobile Testing ($320\text{px} - 767\text{px}$)
- [ ] Announcement bar is legible and fits small viewports.
- [ ] Hamburger menu button opens full-screen editorial mobile overlay.
- [ ] Theme toggle and translation selector are easily tappable in mobile menu.
- [ ] Product cards stack in 1 or 2 columns with easy tap targets.
- [ ] Quantity selector and Add to Cart buttons are comfortably sized for touch ($\ge 44\text{px}$).
- [ ] Cart drawer takes full screen width on small devices.
- [ ] Checkout form fields are single-column with appropriate keyboard input types (`email`, `tel`, `text`).
- [ ] Toast notification appears centered above mobile navigation.

---

## 12. RECOMMENDED IMPLEMENTATION ORDER

To minimize risk and ensure continuous testability, the recommended order of execution across upcoming phases is:

1. **Phase 1 (Theme Architecture):** Establish the CSS variable design tokens and Dark/Light toggle first. This ensures all subsequent component redesigns are built to be theme-aware from the start.
2. **Phase 2 (Navbar & Navigation):** Upgrade the header, sticky behavior, and mobile drawer. This frames the entire user experience.
3. **Phase 3 (Google Translate):** Embed the translation widget early so its styling and DOM integration are accounted for during page redesigns.
4. **Phase 4 (Homepage & Core Sections):** Redesign the Hero, Category, Philosophy, and Footer sections to establish the Ayurique-inspired visual standard.
5. **Phase 5 (Product UI & Catalog):** Implement the dual-image hover product cards, catalog filters, and product detail view.
6. **Phase 6 (Microinteractions & Motion):** Add subtle scroll reveals, button elevations, and reduced-motion accessibility rules.
7. **Phase 7 (Cart, Checkout & Auth Polish):** Apply the visual styling to the Cart Drawer, Cart Page, Checkout 2-step flow, and Login/Register views while strictly preserving underlying logic and Google Form parameter serialization.
8. **Phase 8 (Final QA & Verification):** Perform TypeScript type checks, production builds, and end-to-end multi-device testing.

---

*Plan formulated with strict adherence to zero code modification constraints for the analysis phase.*
