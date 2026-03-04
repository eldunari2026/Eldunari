# Eldunari Pitch Deck — Scroll-Driven Storytelling Redesign

**Date:** 2026-03-04
**Status:** Approved
**Approach:** A — Scroll-Driven Storytelling

## Problem

The current pitch deck feels like a PowerPoint exported to HTML — boxy cards, predictable grid layouts, generic float/orbit animations, and no use of web-native capabilities like scroll-driven effects. It reads as a template, not a premium brand.

## Goals

- Transform from "slides in a browser" to a scroll-driven narrative
- Key stats and messages magnify into focus as the user scrolls
- Remove template aesthetic (borders, grids, generic animations)
- Create editorial breathing room with luxurious whitespace
- Keep it light-mode, clean, and sophisticated (Apple/Notion aesthetic)

## Design Decisions

### Typography
- Hero: `clamp(72px, 10vw, 140px)`, letter-spacing -4px
- Section headers: `clamp(44px, 6vw, 80px)`, weight 300
- Body: 18-19px, line-height 1.8, max-width 560px
- Stats during magnification: 80-120px
- Pull quotes: 36-48px, full viewport width

### Cards & Layout
- Remove all visible borders — use whitespace + subtle shadows
- Use-case cards: 2-column asymmetric with alternating large/small
- Data control grid: staggered masonry-like layout
- Founder cards: horizontal with photo left, credentials right
- Remove dotted grid background

### Scroll Effects (pure JS + CSS, no external libs)
- Stat magnification: 0.6→1.0 scale, 30%→100% opacity at viewport center
- Hero: stagger-reveal with blur-to-sharp
- Cost bars: width animation + number counting
- Quotes: scale-in as they enter center viewport
- Section transitions: soft crossfade

### Pinned Sections
- Problem section: header pins, stats scroll up sequentially
- AI Unlock: header pins, cost bars animate one-by-one

### Animation Overhaul
- Remove: float, orbit, pulse, shimmer (too generic)
- Add: scale-in-center, clip-reveal, blur-to-sharp, counter animation
- All transitions: custom cubic-bezier for spring feel

### Spacing
- Section padding: 160px 80px
- Inner max-width: 1200px (some elements break out)
- Generous whitespace bands between sections

### Color
- All sections white background with subtle warm gradient washes
- Orange accent used sparingly — only for highest-priority elements
- Warmer shadow palette with orange tint

### Micro-interactions
- Cards: scale(1.02) hover instead of translateY
- Smooth nav dot morphing
- Compliance badges: gentle glow
- Custom cubic-bezier easing throughout
