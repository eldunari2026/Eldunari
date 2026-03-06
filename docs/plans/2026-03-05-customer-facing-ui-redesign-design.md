# E2EDoc Customer-Facing UI Redesign

**Date:** 2026-03-05
**Status:** Approved
**Base:** eldunari-pitch-v4.html → eldunari-pitch-v5.html
**Approach:** B (Component Swap) minus pitch steps

## Goal

Transform the pitch-deck-style site into a customer-facing conversion site using patterns from the B2B Premium UI skill library, while keeping all existing content and section order.

## Audience & Conversion

- **Audience:** Potential customers (SMBs, consultants, enterprises)
- **Primary CTA:** "Get in Touch" → email/contact form
- **Conversion points:** Sticky nav CTA + footer CTA section

---

## Typography Change

**Before:** Instrument Serif + Inter + JetBrains Mono
**After:** Plus Jakarta Sans (Type System A — "Bold B2B", HubSpot-inspired)

```css
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');

:root {
  --font-heading: 'Plus Jakarta Sans', sans-serif;
  --font-body: 'Plus Jakarta Sans', sans-serif;
  --fw-normal: 400;
  --fw-medium: 500;
  --fw-semibold: 600;
  --fw-bold: 700;
  --fw-extrabold: 800;
}
```

---

## Section-by-Section Design

### 1. Hero — Daffodil Brand Reveal

- Full-viewport centered layout
- Daffodil SVG draws itself via stroke-dasharray/dashoffset animation (~2s)
- Once path trace completes, fill transitions to `#F78C00` orange
- "E2EDoc.com" fades in beside the daffodil (right side or below depending on viewport)
- Tagline "Automate the Admin. Unlock the Growth." fades in after (~0.5s delay)
- Scroll hint arrow at bottom
- Remove: hero-badge ("AI-First Workflow Automation"), blur-in animation

### 2. Nav — Sticky with CTA (StickyNav pattern)

- Keep: small daffodil logo + "E2EDoc" text (left)
- Remove: dot navigation (pitch-deck artifact)
- Add: nav links — How It Works, Use Cases, Case Studies, About
- Add: "Get in Touch →" button (right, filled `#F78C00`, border-radius 8px)
- Behavior: transparent on load → solid white + shadow on scroll (>20px)
- Mobile: hamburger menu, CTA always visible
- Height: 68px desktop, 56px mobile

### 3. Trust Bar (NEW — after hero)

Compact horizontal strip using compliance badges already in the site:
- GDPR Compliant | ICO Registered | SOC 2 Ready | ISO 27001 Aligned | DPDPA Compliant
- Subtle border-top/bottom, centered, small text
- Light background (#fdfdf8)

### 4. Pitch Section — No Changes

Keep the 01–04 step flow and left/right layout as-is. Only apply:
- Plus Jakarta Sans typography
- Card-lift micro-interactions on step cards

### 5. Use Cases — HubSpotHorizontalTabs

**Replace** the 8-card grid with a tabbed interface:
- Tabs: Tax Filing | Regulatory | Financial | Legal | Insurance | Logistics | Real Estate
- Tax Filing tab is default active (featured use case)
- Each tab panel: icon (in icon container) + title + description + feature bullets
- Active tab: orange underline accent
- Content fades in on tab switch (200ms ease)
- Mobile: tabs become horizontally scrollable
- **Important:** All tab panels must be equally spaced — reduce text if needed for uniform card height

### 6. Problem Section — UI Polish

- Keep all content (stats, quote)
- Add icon containers (orange-tinted, square, 64px) around stat numbers
- Add card-lift hover on stat cards
- Spring easing on magnification animation

### 7. Outsourcing — Enhanced Visual Split

- Left (upside): green-tinted card background (#f0fff4), large "~50%" stat
- Right (downside): warm red-tinted card background (#fff5f5), risk items with icon containers
- Clearer visual divider (vertical line with ⇄ symbol, already present)
- Bottom quote: editorial pull-quote style (larger font, border-left accent)
- Add card-lift micro-interactions

### 8. AI Unlock — Micro-interaction Polish

- Cost bars: add spring easing to width animation
- Add card-lift to whitespace stat cards
- Counter animation already exists — keep

### 9. Data Control — Icon Container Upgrade

- Replace raw emoji icons with `icon-container--md icon-container--orange` wrappers
- Keep 6-card grid layout
- Keep compliance badges at bottom
- Add card-lift on hover

### 10. Case Studies — OnsetCaseStudyToggle

**Replace** side-by-side cards with tabbed toggle:
- Tabs: "Private Equity Fund" | "e2ecomply.com (Tax Filing)"
- Active panel layout: stats column (left) + quote + attribution (right)
- PE Fund stats: 60-70% time savings, $250-350K annual savings
- e2ecomply stats: 50% cost savings, ~5x productivity
- Tab switch: fade animation (250ms)
- Mobile: stacks vertically

### 11. Founders — Micro-interaction Polish

- Keep horizontal card layout
- Add card-lift hover
- Stagger reveal on scroll
- Plus Jakarta Sans typography

### 12. Contact CTA (NEW — OnsetContactCTA pattern)

Full-width dark section before implicit footer:
- Background: #0f0f1a
- Heading: "Ready to get started?" (white, 800 weight, clamp 32-56px)
- Subtext: "No contracts. No onboarding fees. Just results from day one."
- Two buttons:
  - "Get in Touch →" (primary, white bg, dark text, 12px radius)
  - "Learn More" (ghost, white border, white text)
- "Get in Touch" links to mailto or contact form (TBD)

---

## Global Micro-interactions

Applied across all sections:

| Interaction | CSS | Easing |
|-------------|-----|--------|
| Card hover lift | translateY(-4px) + shadow | cubic-bezier(0.34, 1.56, 0.64, 1) |
| Button spring | scale(0.97) on :active | cubic-bezier(0.34, 1.56, 0.64, 1) |
| Scroll reveal | opacity 0→1, translateY 24→0px | cubic-bezier(0.16, 1, 0.3, 1) |
| Stagger delay | 80ms between siblings | — |
| Tab content switch | opacity + translateY 8px | 220ms ease-out |

---

## Color Palette (unchanged)

Keep existing E2EDoc palette:
- Accent: `#F78C00` (orange)
- Accent dark: `#D97700`
- Background: `#ffffff` / `#FDFCFA`
- Text: `#111111` / `#555555` / `#999999`
- Dark section: `#0f0f1a` (contact CTA)

---

## Files

- Source: `eldunari-pitch-v4.html`
- Output: `eldunari-pitch-v5.html` (new version)
- Also copy to: `index.html` after approval
