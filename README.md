# DecodeLabs — Project 1: Static Webpage Design

> **Batch 2026 · Phase I · Quality Gate — APPROVED**
> Frontend Engineering Internship · Week 1 Deliverable

---

## Lighthouse Audit — Desktop · 4 × 100

| Metric | Score |
|---|---|
| ⚡ Performance | **100 / 100** |
| ♿ Accessibility | **100 / 100** |
| ✅ Best Practices | **100 / 100** |
| 🔍 SEO | **100 / 100** |

> Audit conducted via Chrome DevTools Lighthouse · Desktop mode · Cold cache.

---

## W3C Validation

| Validator | Result |
|---|---|
| [W3C Markup Validator](https://validator.w3.org/) | ✅ **0 errors · 0 warnings** |
| [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) | ✅ **0 errors** |

---

## Project Overview

This project is the **Phase I deliverable** of the DecodeLabs Frontend Engineering internship (Batch 2026). It demonstrates mastery of the foundational layer of frontend development: semantic HTML5 architecture, CSS engineering, accessibility, and layout systems — before any framework, JavaScript, or dynamic behaviour is introduced.

The guiding philosophy: **structure before code, clarity before cleverness.**

---

## Technical Architecture

### 1. Semantic HTML5 — Zero Div Soup

The DOM is built exclusively with HTML5 landmark elements. Every structural decision is semantic-first.

```
<body>
  <header>          → Site header (fixed, 80px)
    <nav>           → Primary navigation
  <main>
    <section>       → Hero (100vh)
    <section>       → About (principle cards)
      <article>     → Individual principle cards
    <section>       → Services / content grid
      <article>     → Content cards with <figure>/<figcaption>
    <section>       → Requirements checklist
      <aside>       → Checklist panel
  <footer>
    <nav>           → Footer navigation
    <address>       → Contact information
```

**Heading hierarchy** — sequential, no level skipped:

```
h1  → Hero headline (unique per page)
  h2  → Section titles (About, Services, Checklist, Footer nav)
    h3  → Card titles within sections
```

### 2. CSS Architecture — BEM + DRY + Custom Properties

**Methodology:** Block__Element--Modifier (BEM) across 100% of class names.

**Design tokens** are centralised in `:root` — 70+ custom properties covering the full design system:

```css
:root {
  /* Palette (11 colour tokens)      */
  /* Typography (2 font stacks)      */
  /* Type scale (10 size tokens)     */
  /* Spacing (11 spacing tokens)     */
  /* Layout (6 layout tokens)        */
  /* Borders & Radius (4 tokens)     */
  /* Transitions (2 tokens)          */
}
```

**Zero** inline styles. **Zero** ID selectors used for styling.

### 3. Layout Systems — Grid (macro) + Flexbox (micro)

| Scope | Tool | Specification |
|---|---|---|
| Page sections, content grid | **CSS Grid** | 12-column · `column-gap: 24px` · `row-gap: 60px` |
| Header inner, nav list, buttons, cards | **Flexbox** | Component-level micro-alignment only |

The 12-column grid drives the services section with three span configurations:

```css
.content-grid__item        { grid-column: span 4; }   /* 1/3 width */
.content-grid__item--wide  { grid-column: span 6; }   /* 1/2 width */
```

### 4. Dimensional Specification Compliance

| Component | Requirement | Implementation |
|---|---|---|
| Site Header | `height: 80px · width: 100vw` | `height: var(--header-height)` · `width: 100vw` · `position: fixed` |
| Hero Section | `min-height: 100vh · padding: 120px 0` | `min-height: 100vh` · `padding: calc(120px + 80px)` top (accounts for fixed header) |
| Hero Container | `max-width: 1200px` | `max-width: var(--container-max)` |
| Footer | `min-height: 400px · Drafting Blue` | `min-height: var(--footer-height)` · `background: var(--color-navy)` |

### 5. Image Engineering & CLS Prevention

All five images use the full modern pipeline:

```html
<picture>
  <source srcset="assets/imgN.avif" type="image/avif" />
  <img src="assets/imgN.avif"
       alt="[descriptive alt text]"
       width="560" height="360"
       loading="lazy"
       class="content-card__img" />
</picture>
```

- **Format:** `.avif` (modern, best compression/quality ratio)
- **CLS prevention:** explicit `width` and `height` attributes on every `<img>` — browser reserves layout space before image loads
- **Performance:** `loading="lazy"` on all below-the-fold images
- **Semantic grouping:** every image wrapped in `<figure>` with `<figcaption>`

### 6. Accessibility (A11Y)

| Requirement | Implementation |
|---|---|
| Colour contrast | All text ≥ 4.5:1 on backgrounds (`--color-text-primary: #F0F4FF` = **14:1** on `#0A0F1E`) |
| Landmark labelling | All `<section>` elements carry `aria-labelledby` pointing to their heading |
| Decorative content | All decorative icons and annotations carry `aria-hidden="true"` |
| Navigation | Two `<nav>` elements, each with a unique `aria-label` |
| Interactive focus | `:focus-visible` ring on all focusable elements |
| Reduced motion | `@media (prefers-reduced-motion: reduce)` overrides all transitions/animations |
| Keyboard navigation | `rel="noopener noreferrer"` on all external `target="_blank"` links |
| Semantic form | `<address>` used for contact information in the footer |

### 7. Responsive Strategy

Three breakpoint tiers, mobile-first cascade:

| Breakpoint | Grid behaviour |
|---|---|
| `> 1024px` (Desktop) | 12-col grid · 4-col cards · 6-col wide cards |
| `≤ 1024px` (Tablet) | 12-col grid · 6-col cards · 12-col wide cards |
| `≤ 768px` (Mobile) | All cards full-width · nav hidden · reduced hero padding |

---

## File Structure

```
decodelabs-project1-static-webpage/
├── index.html                  # Main document — semantic HTML5
├── style.css                   # External stylesheet — BEM, DRY, CSS Custom Properties
├── .gitignore                  # Git tracking exclusions (local IDE & OS noise cache)
├── README.md                   # Technical documentation & architecture blueprint
├── audit/
│   └── lighthouse-report.html  # Audited Chrome DevTools Lighthouse report (4 × 100)
└── assets/
├── img1.avif               # HTML/CSS Foundations card image
├── img2.avif               # CSS Layout Systems card image
├── img3.avif               # Quality Gate card image
├── img4.avif               # CSS Engineering Principles card image
└── img5.avif               # IPO Mindset card image
```

> **Autonomy:** the project runs fully offline. No runtime CDN dependency.
> The only network requests at load time are the Google Fonts stylesheet (typography) and the preconnect hints — all other assets are local.

---

## Design System

| Token | Value | Usage |
|---|---|---|
| `--color-night` | `#0A0F1E` | Primary background |
| `--color-navy` | `#0D1B3E` | Footer / dark sections (Drafting Blue) |
| `--color-blue-eng` | `#2563EB` | Primary accent, borders, CTAs |
| `--color-amber` | `#E8D5A3` | Warm highlight, ghost buttons |
| `--color-green` | `#16A34A` | Validation / checklist markers |
| `--color-text-primary` | `#F0F4FF` | Body text (14:1 contrast ratio) |
| `--color-text-secondary` | `#A0AEC0` | Muted text (5.2:1 contrast ratio) |
| `--font-display` | IBM Plex Mono | Headlines, labels, code |
| `--font-body` | Inter | Body copy |

---

## Engineering Principles Applied

**Separation of concerns** — HTML owns structure, CSS owns presentation. Not a single `style=""` attribute exists in the markup.

**DRY (Don't Repeat Yourself)** — Every value used more than once lives in a `:root` custom property. Component styles are defined once and reused via BEM modifiers.

**BEM naming** — All 60+ class names follow strict `Block__Element--Modifier` convention, eliminating specificity conflicts and style ambiguity.

**IPO mindset** — Every layout issue is debugged by stage: Input (assets) → Process (CSS box model) → Output (render). This methodology is demonstrated in the Services section content.

---

## How to Run

No build step. No dependencies. No server required.

```bash
# Clone the repository
git clone [https://github.com/YSB1945/decodelabs-project1-static-webpage.git](https://github.com/YSB1945/decodelabs-project1-static-webpage.git)

# Open in browser
open index.html
# or: double-click index.html in your file explorer

---

## Internship Context

| Field | Value |
|---|---|
| Organisation | DecodeLabs |
| Batch | 2026 |
| Phase | I — Visual Architecture |
| Week | 1 |
| Deliverable | Project 1: Static Webpage Design |
| Status | ✅ Quality Gate — APPROVED |

---

*© 2026 DecodeLabs · Project 1: Static Webpage Design*
