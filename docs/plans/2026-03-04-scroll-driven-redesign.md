# Scroll-Driven Pitch Deck Redesign — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform `eldunari-pitch-v3.html` from a PowerPoint-in-a-browser into a premium scroll-driven narrative experience with magnification effects, editorial typography, borderless cards, and pinned sections.

**Architecture:** Single-file HTML/CSS/JS redesign. All scroll effects use vanilla JS (IntersectionObserver + scroll position tracking). No external libraries. CSS custom properties for theming. Progressive enhancement — content is readable without JS.

**Tech Stack:** HTML5, CSS3 (custom properties, clip-path, backdrop-filter), Vanilla JS (IntersectionObserver, requestAnimationFrame, scroll events)

**Design Doc:** `docs/plans/2026-03-04-scroll-driven-redesign-design.md`

**Target File:** `eldunari-pitch-v3.html`

---

### Task 1: Typography & CSS Variable Overhaul

**Files:**
- Modify: `eldunari-pitch-v3.html` — `:root` variables and base typography (lines 9-48)

**Step 1: Update CSS custom properties**

Replace the `:root` block with refined values:
- Remove `--border` and `--border-light` (going borderless)
- Add `--shadow-soft`, `--shadow-hover`, `--bg-wash` (warm tinted backgrounds)
- Update shadow palette to warmer tones with orange tint

```css
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #FDFCFA;
  --bg-card: #ffffff;
  --bg-wash: rgba(247, 140, 0, 0.02);
  --accent: #F78C00;
  --accent-dim: #F78C0010;
  --accent-mid: #F78C0030;
  --accent-light: #FFF8F0;
  --accent-dark: #D97700;
  --text-primary: #111111;
  --text-secondary: #555555;
  --text-muted: #999999;
  --orange: #F78C00;
  --red-soft: #E5484D;
  --blue: #3B82F6;
  --purple: #8B5CF6;
  --gold: #F59E0B;
  --shadow-soft: 0 2px 20px rgba(247, 140, 0, 0.04), 0 1px 4px rgba(0,0,0,0.02);
  --shadow-hover: 0 8px 30px rgba(247, 140, 0, 0.08), 0 2px 8px rgba(0,0,0,0.03);
  --shadow-lg: 0 16px 50px rgba(247, 140, 0, 0.1), 0 4px 16px rgba(0,0,0,0.04);
  --radius: 20px;
  --ease-spring: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-smooth: cubic-bezier(0.4, 0, 0.2, 1);
}
```

**Step 2: Update base body and section styles**

- Body line-height to 1.7
- Section padding to `160px 80px`
- Section inner max-width stays 1200px
- Remove the `.grid-bg` class entirely (delete the dotted grid background)

**Step 3: Verify in browser**

Open `eldunari-pitch-v3.html` in browser, confirm variables apply and grid background is gone.

**Step 4: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "refactor: overhaul CSS variables and base typography for premium feel"
```

---

### Task 2: Animation System Replacement

**Files:**
- Modify: `eldunari-pitch-v3.html` — keyframes and reveal classes (lines 50-149)

**Step 1: Remove old generic animations**

Delete these keyframe blocks: `float`, `floatSlow`, `pulse`, `shimmer`, `orbit`, `orbitReverse`, `gradientShift`, `drawLine`, `ripple`

Keep: `fadeInUp` (but we'll modify it)

**Step 2: Add new premium animation system**

```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); filter: blur(4px); }
  to { opacity: 1; transform: translateY(0); filter: blur(0); }
}

@keyframes scaleInCenter {
  from { opacity: 0; transform: scale(0.85); filter: blur(6px); }
  to { opacity: 1; transform: scale(1); filter: blur(0); }
}

@keyframes clipRevealLeft {
  from { clip-path: inset(0 100% 0 0); }
  to { clip-path: inset(0 0 0 0); }
}

@keyframes clipRevealRight {
  from { clip-path: inset(0 0 0 100%); }
  to { clip-path: inset(0 0 0 0); }
}

@keyframes countUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes gentlePulse {
  0%, 100% { opacity: 0.4; transform: scale(1); }
  50% { opacity: 0.6; transform: scale(1.05); }
}

@keyframes heroBlurIn {
  0% { opacity: 0; filter: blur(12px); transform: scale(1.1); }
  100% { opacity: 1; filter: blur(0); transform: scale(1); }
}
```

**Step 3: Update reveal classes**

Replace existing `.reveal`, `.reveal-left`, `.reveal-right`, `.reveal-scale` with blur-based versions:

```css
.reveal {
  opacity: 0;
  transform: translateY(30px);
  filter: blur(4px);
  transition: opacity 0.9s var(--ease-spring), transform 0.9s var(--ease-spring), filter 0.9s var(--ease-spring);
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
  filter: blur(0);
}

.reveal-left {
  opacity: 0;
  transform: translateX(-40px);
  filter: blur(4px);
  transition: opacity 0.9s var(--ease-spring), transform 0.9s var(--ease-spring), filter 0.9s var(--ease-spring);
}
.reveal-left.visible {
  opacity: 1;
  transform: translateX(0);
  filter: blur(0);
}

.reveal-right {
  opacity: 0;
  transform: translateX(40px);
  filter: blur(4px);
  transition: opacity 0.9s var(--ease-spring), transform 0.9s var(--ease-spring), filter 0.9s var(--ease-spring);
}
.reveal-right.visible {
  opacity: 1;
  transform: translateX(0);
  filter: blur(0);
}

.reveal-scale {
  opacity: 0;
  transform: scale(0.85);
  filter: blur(6px);
  transition: opacity 0.9s var(--ease-spring), transform 0.9s var(--ease-spring), filter 0.9s var(--ease-spring);
}
.reveal-scale.visible {
  opacity: 1;
  transform: scale(1);
  filter: blur(0);
}
```

**Step 4: Add magnification class**

```css
.magnify {
  opacity: 0.3;
  transform: scale(0.6);
  filter: blur(3px);
  transition: opacity 0.8s var(--ease-spring), transform 0.8s var(--ease-spring), filter 0.8s var(--ease-spring);
}
.magnify.in-focus {
  opacity: 1;
  transform: scale(1);
  filter: blur(0);
}
```

**Step 5: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "refactor: replace generic animations with premium blur-reveal system"
```

---

### Task 3: Hero Section Redesign

**Files:**
- Modify: `eldunari-pitch-v3.html` — Hero CSS (lines 229-341) and Hero HTML (lines 1306-1319)

**Step 1: Replace hero CSS**

Remove the orb container, orb animations, and grid-bg references. New hero:

```css
#hero {
  text-align: center;
  background: var(--bg-primary);
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
#hero::before {
  content: '';
  position: absolute;
  top: -30%;
  left: 50%;
  transform: translateX(-50%);
  width: 800px;
  height: 800px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(247, 140, 0, 0.06) 0%, transparent 70%);
  animation: gentlePulse 6s ease-in-out infinite;
  pointer-events: none;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 24px;
  border-radius: 40px;
  background: var(--accent-light);
  font-size: 11px;
  font-weight: 600;
  color: var(--accent);
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 48px;
  animation: heroBlurIn 1s var(--ease-spring) 0.2s both;
}
.hero-badge::before {
  content: '';
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--accent);
}

.hero-title {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(72px, 10vw, 140px);
  line-height: 0.95;
  letter-spacing: -4px;
  margin-bottom: 16px;
  color: var(--text-primary);
}
.hero-title .accent {
  color: var(--accent);
}
.hero-title-word {
  display: inline-block;
  animation: heroBlurIn 0.8s var(--ease-spring) both;
}
.hero-title-word:nth-child(1) { animation-delay: 0.3s; }
.hero-title-word:nth-child(2) { animation-delay: 0.5s; }

.hero-tagline {
  font-size: clamp(22px, 3vw, 32px);
  color: var(--text-secondary);
  font-weight: 300;
  margin-top: 32px;
  letter-spacing: -0.5px;
  animation: heroBlurIn 0.8s var(--ease-spring) 0.7s both;
}
.hero-tagline em {
  font-style: italic;
  font-family: 'Instrument Serif', serif;
  color: var(--text-primary);
  font-size: 1.08em;
}

.hero-line {
  width: 48px;
  height: 2px;
  background: var(--accent);
  margin: 48px auto 0;
  animation: heroBlurIn 0.8s var(--ease-spring) 0.9s both;
  border-radius: 1px;
}

.hero-scroll-hint {
  position: absolute;
  bottom: 48px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  animation: heroBlurIn 1s var(--ease-spring) 1.2s both;
}
.hero-scroll-hint span {
  font-size: 11px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--text-muted);
  font-weight: 500;
}
.hero-scroll-arrow {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, var(--text-muted), transparent);
  animation: heroScrollPulse 2s ease-in-out infinite;
}
@keyframes heroScrollPulse {
  0%, 100% { opacity: 0.3; transform: scaleY(0.7); }
  50% { opacity: 0.8; transform: scaleY(1); }
}
```

**Step 2: Update hero HTML**

Remove orb container and grid-bg div. Update to:

```html
<section id="hero">
  <div class="section-inner" style="text-align:center;">
    <div class="hero-badge">AI-First Workflow Automation</div>
    <h1 class="hero-title">
      <span class="hero-title-word accent">Eldunari</span><span class="hero-title-word">.ai</span>
    </h1>
    <p class="hero-tagline">Automate the Admin. <em>Unlock the Growth.</em></p>
    <div class="hero-line"></div>
  </div>
  <div class="hero-scroll-hint">
    <span>Scroll</span>
    <div class="hero-scroll-arrow"></div>
  </div>
</section>
```

**Step 3: Remove all `.grid-bg` divs from every section in HTML**

Remove every `<div class="grid-bg"></div>` from the HTML.

**Step 4: Remove `.grid-bg` CSS class**

Delete the `.grid-bg` CSS block.

**Step 5: Remove hero orb CSS**

Delete `.hero-orb-container`, `.hero-orb`, `.hero-orb-1`, `.hero-orb-2`, `.hero-orb-3` CSS blocks.

**Step 6: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign hero section with blur-in title and scroll hint"
```

---

### Task 4: Pitch Section (Slide 2) — Editorial Layout

**Files:**
- Modify: `eldunari-pitch-v3.html` — Pitch CSS (lines 342-453) and Pitch HTML (lines 1321-1357)

**Step 1: Redesign pitch CSS**

Replace the floating node visualization with a cleaner approach. Keep the 2-column layout but make it more editorial:

```css
#pitch {
  background: var(--bg-secondary);
  position: relative;
}
#pitch::before {
  content: '';
  position: absolute;
  top: 0; right: -200px;
  width: 600px; height: 600px;
  border-radius: 50%;
  background: radial-gradient(circle, var(--bg-wash) 0%, transparent 70%);
  pointer-events: none;
}

.pitch-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 100px;
  align-items: center;
}

.pitch-left h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(40px, 5vw, 64px);
  line-height: 1.1;
  letter-spacing: -2px;
  margin-bottom: 32px;
  color: var(--text-primary);
  font-weight: 400;
}
.pitch-left h2 .highlight {
  color: var(--accent);
}

.pitch-left p {
  font-size: 18px;
  color: var(--text-secondary);
  line-height: 1.8;
  max-width: 480px;
}

.pitch-steps {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.pitch-step {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 24px 28px;
  background: var(--bg-card);
  border-radius: var(--radius);
  box-shadow: var(--shadow-soft);
  transition: all 0.4s var(--ease-spring);
}
.pitch-step:hover {
  box-shadow: var(--shadow-hover);
  transform: scale(1.02);
}

.pitch-step-num {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'JetBrains Mono', monospace;
  font-size: 14px;
  font-weight: 600;
  flex-shrink: 0;
}
.pitch-step-1 .pitch-step-num { background: rgba(247, 140, 0, 0.08); color: var(--accent); }
.pitch-step-2 .pitch-step-num { background: rgba(59, 130, 246, 0.08); color: var(--blue); }
.pitch-step-3 .pitch-step-num { background: rgba(139, 92, 246, 0.08); color: var(--purple); }
.pitch-step-4 .pitch-step-num { background: rgba(247, 140, 0, 0.08); color: var(--accent); }

.pitch-step-text h4 {
  font-size: 15px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 2px;
}
.pitch-step-text p {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.5;
}
```

**Step 2: Replace pitch HTML**

Replace the floating node visual with clean numbered steps:

```html
<section id="pitch">
  <div class="section-inner">
    <div class="pitch-layout">
      <div class="pitch-left reveal-left">
        <h2>We automate <span class="highlight">Knowledge Entry</span> workflows with an AI-first approach</h2>
        <p>Businesses pour enormous time and money into repetitive data input — extracting information from documents, entering it into systems, validating and reconciling. Eldunari replaces these manual workflows with intelligent AI agents.</p>
      </div>
      <div class="pitch-steps reveal-right">
        <div class="pitch-step pitch-step-1">
          <div class="pitch-step-num">01</div>
          <div class="pitch-step-text">
            <h4>Documents In</h4>
            <p>Ingest PDFs, scans, emails, spreadsheets</p>
          </div>
        </div>
        <div class="pitch-step pitch-step-2">
          <div class="pitch-step-num">02</div>
          <div class="pitch-step-text">
            <h4>AI Extraction</h4>
            <p>Intelligent data extraction and structuring</p>
          </div>
        </div>
        <div class="pitch-step pitch-step-3">
          <div class="pitch-step-num">03</div>
          <div class="pitch-step-text">
            <h4>Smart Validation</h4>
            <p>Cross-reference and verify against rules</p>
          </div>
        </div>
        <div class="pitch-step pitch-step-4">
          <div class="pitch-step-num">04</div>
          <div class="pitch-step-text">
            <h4>Auto-Entry</h4>
            <p>Populate target systems automatically</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

**Step 3: Remove old pitch node/connector/orb CSS**

Delete: `.pitch-node`, `.pitch-node-icon`, `.node-docs`, `.node-extract`, `.node-validate`, `.node-enter`, `.pitch-center-orb`, `.pitch-connector` and all related CSS.

**Step 4: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign pitch section with editorial layout and numbered steps"
```

---

### Task 5: Use Cases — Asymmetric Grid

**Files:**
- Modify: `eldunari-pitch-v3.html` — Use cases CSS (lines 455-541) and HTML (lines 1359-1410)

**Step 1: Replace use cases CSS**

Convert from 4-column grid to a more interesting 2-column asymmetric layout with larger feature cards:

```css
#usecases {
  background: var(--bg-primary);
}

.usecases-header {
  text-align: center;
  margin-bottom: 80px;
}
.usecases-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
}
.usecases-header p {
  color: var(--text-secondary);
  font-size: 18px;
}

.usecases-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.usecase-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 36px 32px;
  transition: all 0.4s var(--ease-spring);
  position: relative;
  overflow: hidden;
  cursor: default;
  box-shadow: var(--shadow-soft);
}
.usecase-card:hover {
  transform: scale(1.02);
  box-shadow: var(--shadow-hover);
}

.usecase-icon {
  width: 56px;
  height: 56px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  margin-bottom: 20px;
  background: var(--accent-light);
}

.usecase-card h3 {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 8px;
  letter-spacing: -0.3px;
  color: var(--text-primary);
}
.usecase-card p {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.7;
}

/* Feature cards span full width for variety */
.usecase-card.featured {
  grid-column: span 2;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 24px;
  align-items: center;
  padding: 40px 36px;
}
.usecase-card.featured .usecase-icon {
  margin-bottom: 0;
  width: 64px;
  height: 64px;
  font-size: 28px;
}
.usecase-card.featured h3 {
  font-size: 18px;
}
```

**Step 2: Update use cases HTML — make first and last cards featured**

```html
<section id="usecases">
  <div class="section-inner">
    <div class="usecases-header reveal">
      <h2>One capability.<br>Endless applications.</h2>
      <p>Knowledge Entry powers workflows across every industry</p>
    </div>
    <div class="usecases-grid">
      <div class="usecase-card featured reveal stagger-1">
        <div class="usecase-icon" style="background:rgba(247,140,0,0.08);">🧾</div>
        <div>
          <h3>Tax Filing</h3>
          <p>Income, expenses & deductions extracted and entered into returns automatically</p>
        </div>
      </div>
      <div class="usecase-card reveal stagger-2">
        <div class="usecase-icon" style="background:rgba(59,130,246,0.08);">📋</div>
        <h3>Regulatory & Compliance</h3>
        <p>Periodic submissions to regulators — RBI, SEBI, HMRC, SEC and more</p>
      </div>
      <div class="usecase-card reveal stagger-3">
        <div class="usecase-icon" style="background:rgba(139,92,246,0.08);">📊</div>
        <h3>Financial Reporting</h3>
        <p>Journal entries, trial balance prep & consolidation data input</p>
      </div>
      <div class="usecase-card reveal stagger-4">
        <div class="usecase-icon" style="background:rgba(247,140,0,0.08);">⚖️</div>
        <h3>Legal & Contracts</h3>
        <p>Clause extraction, contract metadata entry & IP filings</p>
      </div>
      <div class="usecase-card reveal stagger-5">
        <div class="usecase-icon" style="background:rgba(245,158,11,0.08);">🛡️</div>
        <h3>Insurance Claims</h3>
        <p>Claims capture, policy details & adjudication preparation</p>
      </div>
      <div class="usecase-card reveal stagger-6">
        <div class="usecase-icon" style="background:rgba(229,72,77,0.08);">🚚</div>
        <h3>Logistics & Supply Chain</h3>
        <p>Purchase orders, invoices & customs documentation</p>
      </div>
      <div class="usecase-card reveal stagger-7">
        <div class="usecase-icon" style="background:rgba(59,130,246,0.08);">🏢</div>
        <h3>Real Estate & Property</h3>
        <p>Lease data, tenant records & valuation inputs</p>
      </div>
      <div class="usecase-card reveal stagger-8" style="display:flex;flex-direction:column;align-items:center;justify-content:center;background:var(--bg-secondary);">
        <div style="font-size:32px;margin-bottom:12px;opacity:0.2;">+</div>
        <h3 style="color:var(--text-muted);">And More</h3>
        <p>Any workflow involving structured data entry from unstructured sources</p>
      </div>
    </div>
  </div>
</section>
```

**Step 3: Remove old usecase-specific color classes from CSS**

Delete `.uc-tax`, `.uc-compliance`, `.uc-finance`, `.uc-legal`, `.uc-insurance`, `.uc-logistics`, `.uc-realestate` CSS blocks (colors now inline).

**Step 4: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign use cases with asymmetric grid and borderless cards"
```

---

### Task 6: Problem Section — Magnification Stats

**Files:**
- Modify: `eldunari-pitch-v3.html` — Problem CSS (lines 542-622) and HTML (lines 1412-1435)

**Step 1: Update problem section CSS**

```css
#problem {
  background: var(--bg-secondary);
  position: relative;
}

.problem-header {
  text-align: center;
  margin-bottom: 80px;
}
.problem-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
  line-height: 1.1;
}
.problem-header p {
  color: var(--text-secondary);
  font-size: 18px;
}
.problem-header .target-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 20px;
  border-radius: 40px;
  background: var(--accent-light);
  font-size: 12px;
  color: var(--accent);
  font-weight: 600;
  margin-bottom: 32px;
  letter-spacing: 0.5px;
}

.problem-stats {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
  max-width: 800px;
  margin: 0 auto 72px;
}

.stat-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 48px;
  text-align: center;
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-soft);
}

.stat-number {
  font-family: 'JetBrains Mono', monospace;
  font-size: clamp(56px, 7vw, 96px);
  font-weight: 500;
  color: var(--accent);
  line-height: 1;
  margin-bottom: 16px;
}
.stat-label {
  font-size: 16px;
  color: var(--text-secondary);
  font-weight: 400;
  line-height: 1.6;
}

.problem-quote {
  text-align: center;
  font-family: 'Instrument Serif', serif;
  font-style: italic;
  font-size: clamp(24px, 3vw, 36px);
  color: var(--text-secondary);
  max-width: 800px;
  margin: 0 auto;
  line-height: 1.5;
}
.problem-quote span {
  color: var(--accent);
  font-weight: 500;
}
```

**Step 2: Add `magnify` class to stat cards and quote in HTML**

Update the problem section HTML — add `magnify` class alongside `reveal`:

```html
<section id="problem">
  <div class="section-inner">
    <div class="problem-header reveal">
      <div class="target-badge">🎯 SMBs & Independent Consultants</div>
      <h2>Your best people are stuck<br>doing data entry</h2>
      <p>The hours and money lost to manual knowledge entry are staggering</p>
    </div>
    <div class="problem-stats">
      <div class="stat-card magnify stagger-1">
        <div class="stat-number">~20%</div>
        <div class="stat-label">of productive time consumed by<br>knowledge entry tasks</div>
      </div>
      <div class="stat-card magnify stagger-2">
        <div class="stat-number">~30%</div>
        <div class="stat-label">of operational costs tied to admin<br>data workflows</div>
      </div>
    </div>
    <div class="problem-quote magnify stagger-3">
      Every hour spent on data entry is an hour <span>not</span> spent on business development, client relationships, or strategic growth.
    </div>
  </div>
</section>
```

**Step 3: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign problem section with magnification stats"
```

---

### Task 7: Outsourcing Section — Cleaner Comparison

**Files:**
- Modify: `eldunari-pitch-v3.html` — Outsourcing CSS (lines 624-735) and HTML (lines 1437-1481)

**Step 1: Update outsourcing CSS**

Remove borders, use shadows and background differentiation:

```css
#outsourcing {
  background: var(--bg-primary);
}

.outsource-header {
  text-align: center;
  margin-bottom: 72px;
}
.outsource-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
  line-height: 1.1;
}
.outsource-header p { color: var(--text-secondary); font-size: 18px; }

.outsource-layout {
  display: grid;
  grid-template-columns: 1fr 60px 1fr;
  gap: 0;
  align-items: start;
  max-width: 900px;
  margin: 0 auto;
}

.outsource-side {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 44px;
  box-shadow: var(--shadow-soft);
  transition: all 0.4s var(--ease-spring);
}
.outsource-side:hover {
  box-shadow: var(--shadow-hover);
}

.outsource-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  color: var(--text-muted);
  padding-top: 60px;
}

.outsource-good h3 {
  color: var(--accent);
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 24px;
  font-weight: 600;
}
.outsource-good .big-stat {
  font-family: 'JetBrains Mono', monospace;
  font-size: 56px;
  color: var(--accent);
  font-weight: 500;
  margin-bottom: 8px;
}
.outsource-good .big-label {
  color: var(--text-secondary);
  font-size: 15px;
  line-height: 1.6;
}

.outsource-bad h3 {
  color: var(--red-soft);
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 28px;
  font-weight: 600;
}

.outsource-risk {
  display: flex;
  align-items: flex-start;
  gap: 16px;
  margin-bottom: 28px;
}
.outsource-risk:last-child { margin-bottom: 0; }
.outsource-risk-icon {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: rgba(229, 72, 77, 0.05);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  flex-shrink: 0;
  margin-top: 2px;
}
.outsource-risk h4 {
  font-size: 15px;
  font-weight: 600;
  margin-bottom: 4px;
  color: var(--text-primary);
}
.outsource-risk p {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.6;
}

.outsource-bottom {
  text-align: center;
  margin-top: 56px;
  font-family: 'Instrument Serif', serif;
  font-style: italic;
  font-size: clamp(20px, 2.5vw, 28px);
  color: var(--text-secondary);
}
```

**Step 2: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign outsourcing section with borderless cards"
```

---

### Task 8: AI Unlock Section — Animated Cost Bars

**Files:**
- Modify: `eldunari-pitch-v3.html` — AI Unlock CSS (lines 736-831) and HTML (lines 1483-1526)

**Step 1: Update AI unlock CSS**

```css
#ai-unlock {
  background: var(--bg-secondary);
  position: relative;
}

.ai-header {
  text-align: center;
  margin-bottom: 72px;
}
.ai-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
  line-height: 1.1;
}
.ai-header p { color: var(--text-secondary); font-size: 18px; max-width: 560px; margin: 0 auto; }

.cost-bars {
  max-width: 700px;
  margin: 0 auto 64px;
  display: flex;
  flex-direction: column;
  gap: 28px;
}

.cost-bar-row {
  display: grid;
  grid-template-columns: 160px 1fr 80px;
  align-items: center;
  gap: 24px;
}
.cost-bar-label {
  font-size: 15px;
  font-weight: 500;
  color: var(--text-secondary);
  text-align: right;
}
.cost-bar-track {
  height: 44px;
  border-radius: 12px;
  background: rgba(0,0,0,0.03);
  overflow: hidden;
  position: relative;
}
.cost-bar-fill {
  height: 100%;
  border-radius: 12px;
  display: flex;
  align-items: center;
  padding-left: 18px;
  font-size: 13px;
  font-weight: 500;
  color: white;
  transition: width 1.8s var(--ease-spring);
  width: 0;
}
.bar-onshore { background: linear-gradient(90deg, #E5484D, #F76B6B); }
.bar-offshore { background: linear-gradient(90deg, #E8900C, #F5AD3B); }
.bar-ai { background: linear-gradient(90deg, #F78C00, #FFAB40); }
.cost-bar-pct {
  font-family: 'JetBrains Mono', monospace;
  font-size: 18px;
  font-weight: 500;
}
.pct-red { color: var(--red-soft); }
.pct-orange { color: var(--accent-dark); }
.pct-green { color: var(--accent); }

.ai-whitespace {
  text-align: center;
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 40px 48px;
  max-width: 700px;
  margin: 0 auto;
  display: flex;
  gap: 48px;
  justify-content: center;
  box-shadow: var(--shadow-soft);
}
.ws-stat .ws-num {
  font-family: 'JetBrains Mono', monospace;
  font-size: 36px;
  font-weight: 500;
  color: var(--accent);
}
.ws-stat .ws-label {
  font-size: 14px;
  color: var(--text-muted);
  margin-top: 6px;
  line-height: 1.5;
}
```

**Step 2: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign AI unlock section with refined cost bars"
```

---

### Task 9: Data Control Section — Staggered Layout

**Files:**
- Modify: `eldunari-pitch-v3.html` — Data Control CSS (lines 833-924) and HTML (lines 1528-1576)

**Step 1: Update data control CSS**

```css
#datacontrol {
  background: var(--bg-primary);
  position: relative;
}

.dc-header {
  text-align: center;
  margin-bottom: 72px;
}
.dc-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
}
.dc-header p { color: var(--text-secondary); font-size: 18px; max-width: 600px; margin: 0 auto; }

.dc-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-bottom: 56px;
}

.dc-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 32px;
  position: relative;
  overflow: hidden;
  transition: all 0.4s var(--ease-spring);
  box-shadow: var(--shadow-soft);
}
.dc-card:hover {
  transform: scale(1.02);
  box-shadow: var(--shadow-hover);
}
.dc-card-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  margin-bottom: 20px;
  background: var(--accent-light);
}
.dc-card h3 {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 8px;
  letter-spacing: -0.3px;
  color: var(--text-primary);
}
.dc-card p {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.7;
}

/* Make first card span 2 columns for visual interest */
.dc-card:first-child {
  grid-column: span 2;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 24px;
  align-items: start;
}
.dc-card:first-child .dc-card-icon {
  margin-bottom: 0;
  width: 56px;
  height: 56px;
  font-size: 26px;
}

.dc-compliance {
  display: flex;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
}
.compliance-badge {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  border-radius: 40px;
  background: var(--bg-secondary);
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
  transition: all 0.3s var(--ease-spring);
  box-shadow: var(--shadow-soft);
}
.compliance-badge:hover {
  color: var(--accent);
  box-shadow: var(--shadow-hover);
}
.compliance-badge .badge-dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--accent);
}
```

**Step 2: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: redesign data control section with staggered card layout"
```

---

### Task 10: Case Studies & Founders — Polish

**Files:**
- Modify: `eldunari-pitch-v3.html` — Cases CSS (lines 926-1045) and Founders CSS (lines 1047-1231)

**Step 1: Update case studies CSS**

Remove borders, use shadows, increase type sizes:

```css
#cases {
  background: var(--bg-secondary);
}

.cases-header {
  text-align: center;
  margin-bottom: 72px;
}
.cases-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
}
.cases-header p { color: var(--text-secondary); font-size: 18px; }

.cases-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 28px;
}

.case-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 40px;
  position: relative;
  overflow: hidden;
  transition: all 0.4s var(--ease-spring);
  box-shadow: var(--shadow-soft);
}
.case-card:hover {
  box-shadow: var(--shadow-hover);
  transform: scale(1.01);
}
.case-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
}
.case-1::before { background: linear-gradient(90deg, var(--blue), var(--purple)); }
.case-2::before { background: linear-gradient(90deg, var(--accent), var(--gold)); }

.case-badge {
  display: inline-flex;
  padding: 5px 14px;
  border-radius: 8px;
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  margin-bottom: 20px;
}
.case-1 .case-badge { background: rgba(59, 130, 246, 0.06); color: var(--blue); }
.case-2 .case-badge { background: var(--accent-light); color: var(--accent); }

.case-card h3 {
  font-family: 'Instrument Serif', serif;
  font-size: 26px;
  margin-bottom: 8px;
  letter-spacing: -0.5px;
  color: var(--text-primary);
}
.case-card .case-desc {
  font-size: 15px;
  color: var(--text-muted);
  margin-bottom: 28px;
  line-height: 1.7;
}

.case-workflows { margin-bottom: 28px; }
.case-workflows h4 {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: var(--text-muted);
  margin-bottom: 12px;
}
.case-workflow-tags { display: flex; flex-wrap: wrap; gap: 8px; }
.case-tag {
  padding: 6px 14px;
  border-radius: 8px;
  background: var(--bg-secondary);
  font-size: 12px;
  color: var(--text-secondary);
}

.case-impact {
  display: flex;
  gap: 24px;
  padding-top: 28px;
  border-top: 1px solid rgba(0,0,0,0.05);
}
.case-impact-stat { text-align: center; flex: 1; }
.case-impact-num {
  font-family: 'JetBrains Mono', monospace;
  font-size: 32px;
  font-weight: 500;
  line-height: 1;
  margin-bottom: 6px;
}
.case-1 .case-impact-num { color: var(--blue); }
.case-2 .case-impact-num { color: var(--accent); }
.case-impact-label { font-size: 12px; color: var(--text-muted); }
```

**Step 2: Update founders CSS**

```css
#founders {
  background: var(--bg-primary);
}

.founders-header {
  text-align: center;
  margin-bottom: 80px;
}
.founders-header h2 {
  font-family: 'Instrument Serif', serif;
  font-size: clamp(44px, 6vw, 72px);
  letter-spacing: -2px;
  margin-bottom: 16px;
  font-weight: 400;
}
.founders-header p {
  color: var(--text-secondary);
  font-size: 18px;
  max-width: 600px;
  margin: 0 auto;
}

.founders-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
  max-width: 900px;
  margin: 0 auto 56px;
}

.founder-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 44px 40px;
  position: relative;
  overflow: hidden;
  transition: all 0.4s var(--ease-spring);
  box-shadow: var(--shadow-soft);
  display: flex;
  flex-direction: column;
}
.founder-card:hover {
  box-shadow: var(--shadow-hover);
  transform: scale(1.01);
}
.founder-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--accent), var(--gold));
}

.founder-photo {
  width: 84px;
  height: 84px;
  border-radius: 50%;
  background: var(--accent-light);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 24px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(247, 140, 0, 0.1);
}
.founder-photo-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  color: var(--accent);
  font-family: 'Instrument Serif', serif;
  font-weight: 500;
  background: linear-gradient(135deg, var(--accent-light), rgba(247, 140, 0, 0.15));
}
.founder-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.founder-role {
  display: inline-flex;
  padding: 5px 14px;
  border-radius: 8px;
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  margin-bottom: 16px;
  background: var(--accent-light);
  color: var(--accent);
}

.founder-card h3 {
  font-family: 'Instrument Serif', serif;
  font-size: 24px;
  letter-spacing: -0.5px;
  margin-bottom: 24px;
  color: var(--text-primary);
}

.founder-credentials {
  display: flex;
  flex-direction: column;
  gap: 14px;
  margin-bottom: 24px;
  flex: 1;
}

.founder-credential {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.5;
}
.founder-credential-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  flex-shrink: 0;
  margin-top: 1px;
  background: var(--bg-secondary);
}

.founder-education {
  padding-top: 20px;
  border-top: 1px solid rgba(0,0,0,0.05);
}
.founder-education-label {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: var(--text-muted);
  margin-bottom: 10px;
  font-weight: 600;
}
.founder-education-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.founder-edu-tag {
  padding: 5px 12px;
  border-radius: 8px;
  background: var(--bg-secondary);
  font-size: 12px;
  color: var(--text-secondary);
  font-weight: 500;
}

.founders-summary {
  text-align: center;
  max-width: 700px;
  margin: 0 auto;
  padding: 36px 44px;
  background: var(--bg-secondary);
  border-radius: var(--radius);
  box-shadow: var(--shadow-soft);
}
.founders-summary p {
  font-family: 'Instrument Serif', serif;
  font-style: italic;
  font-size: clamp(18px, 2.5vw, 24px);
  color: var(--text-secondary);
  line-height: 1.6;
}
.founders-summary p span {
  color: var(--accent);
  font-style: normal;
  font-weight: 500;
}
```

**Step 3: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: polish case studies and founders sections"
```

---

### Task 11: Navigation & Responsive Updates

**Files:**
- Modify: `eldunari-pitch-v3.html` — Nav CSS (lines 150-209), responsive (lines 1254-1269)

**Step 1: Update nav CSS**

```css
nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 20px 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
}

.nav-logo {
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
}
.nav-logo svg {
  width: 36px;
  height: 36px;
}
.nav-logo-text {
  font-family: 'Instrument Serif', serif;
  font-size: 22px;
  color: var(--text-primary);
  letter-spacing: -0.5px;
}
.nav-logo-text span {
  color: var(--text-muted);
  font-size: 14px;
  font-family: 'Inter', sans-serif;
  margin-left: 2px;
  font-weight: 400;
}

.nav-dots {
  display: flex;
  gap: 8px;
}
.nav-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(0,0,0,0.1);
  transition: all 0.5s var(--ease-spring);
  cursor: pointer;
  border: none;
}
.nav-dot.active {
  background: var(--accent);
  width: 28px;
  border-radius: 4px;
  box-shadow: 0 0 12px rgba(247, 140, 0, 0.3);
}

.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--accent), var(--gold));
  z-index: 200;
  transition: width 0.1s linear;
}
```

**Step 2: Update responsive CSS**

```css
@media (max-width: 900px) {
  section { padding: 100px 24px; }
  nav { padding: 16px 24px; }
  .pitch-layout { grid-template-columns: 1fr; gap: 48px; }
  .usecases-grid { grid-template-columns: 1fr; }
  .usecase-card.featured { grid-column: span 1; display: flex; flex-direction: column; }
  .problem-stats { grid-template-columns: 1fr; }
  .outsource-layout { grid-template-columns: 1fr; }
  .outsource-divider { padding: 20px 0; transform: rotate(90deg); }
  .dc-grid { grid-template-columns: 1fr; }
  .dc-card:first-child { grid-column: span 1; display: flex; flex-direction: column; }
  .cases-grid { grid-template-columns: 1fr; }
  .cost-bar-row { grid-template-columns: 100px 1fr 60px; gap: 12px; }
  .ai-whitespace { flex-direction: column; gap: 24px; }
  .founders-grid { grid-template-columns: 1fr; }
}
```

**Step 3: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: update nav and responsive styles for premium feel"
```

---

### Task 12: JavaScript — Magnification Engine & Scroll Effects

**Files:**
- Modify: `eldunari-pitch-v3.html` — `<script>` block (lines 1725-1791)

**Step 1: Replace the entire script block**

```javascript
// ==================== SCROLL REVEAL ====================
const observerOptions = {
  threshold: 0.15,
  rootMargin: '0px 0px -40px 0px'
};

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    }
  });
}, observerOptions);

document.querySelectorAll('.reveal, .reveal-left, .reveal-right, .reveal-scale').forEach(el => {
  observer.observe(el);
});

// ==================== MAGNIFICATION ENGINE ====================
// Elements with .magnify scale from 0.6/30% opacity to full as they
// reach the center of the viewport
function updateMagnification() {
  const magnifyEls = document.querySelectorAll('.magnify');
  const viewportCenter = window.innerHeight / 2;

  magnifyEls.forEach(el => {
    const rect = el.getBoundingClientRect();
    const elCenter = rect.top + rect.height / 2;
    const distanceFromCenter = Math.abs(elCenter - viewportCenter);
    const maxDistance = window.innerHeight * 0.5;

    // Calculate progress: 1 when at center, 0 when far away
    const progress = Math.max(0, 1 - (distanceFromCenter / maxDistance));

    if (progress > 0.4) {
      el.classList.add('in-focus');
    } else {
      el.classList.remove('in-focus');
    }
  });

  requestAnimationFrame(updateMagnification);
}
requestAnimationFrame(updateMagnification);

// ==================== COST BAR ANIMATION ====================
const barObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const fills = entry.target.querySelectorAll('.cost-bar-fill');
      fills.forEach((fill, i) => {
        setTimeout(() => {
          fill.style.width = fill.dataset.width;
        }, i * 400);
      });
    }
  });
}, { threshold: 0.3 });

const costBars = document.getElementById('costBars');
if (costBars) barObserver.observe(costBars);

// ==================== SCROLL PROGRESS ====================
window.addEventListener('scroll', () => {
  const scrollTop = window.scrollY;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  const scrollPercent = (scrollTop / docHeight) * 100;
  document.getElementById('scrollProgress').style.width = scrollPercent + '%';
}, { passive: true });

// ==================== NAV DOTS ====================
const sections = document.querySelectorAll('section');
const navDots = document.querySelectorAll('.nav-dot');

navDots.forEach(dot => {
  dot.addEventListener('click', () => {
    const target = document.getElementById(dot.dataset.section);
    if (target) target.scrollIntoView({ behavior: 'smooth' });
  });
});

const sectionObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      navDots.forEach(d => d.classList.remove('active'));
      const activeDot = document.querySelector(`.nav-dot[data-section="${entry.target.id}"]`);
      if (activeDot) activeDot.classList.add('active');
    }
  });
}, { threshold: 0.4 });

sections.forEach(s => sectionObserver.observe(s));

// ==================== COUNTER ANIMATION ====================
// Animate stat numbers counting up when they come into view
function animateCounter(el, target, duration) {
  const isPercent = target.includes('%');
  const isTilde = target.startsWith('~');
  const prefix = isTilde ? '~' : '';
  const numStr = target.replace(/[^0-9.]/g, '');
  const num = parseFloat(numStr);
  const suffix = target.replace(/[0-9.~]/g, '').trim();
  const startTime = performance.now();

  function update(currentTime) {
    const elapsed = currentTime - startTime;
    const progress = Math.min(elapsed / duration, 1);
    // Ease out cubic
    const eased = 1 - Math.pow(1 - progress, 3);
    const current = Math.round(num * eased * 10) / 10;

    let display = prefix;
    if (Number.isInteger(num)) {
      display += Math.round(num * eased);
    } else {
      display += (num * eased).toFixed(1);
    }
    display += suffix;
    el.textContent = display;

    if (progress < 1) {
      requestAnimationFrame(update);
    } else {
      el.textContent = target;
    }
  }
  requestAnimationFrame(update);
}

const counterObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting && !entry.target.dataset.counted) {
      entry.target.dataset.counted = 'true';
      const target = entry.target.textContent;
      animateCounter(entry.target, target, 1500);
    }
  });
}, { threshold: 0.5 });

document.querySelectorAll('.stat-number, .big-stat, .ws-num, .case-impact-num').forEach(el => {
  counterObserver.observe(el);
});
```

**Step 2: Commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "feat: add magnification engine, counter animation, and scroll effects"
```

---

### Task 13: Final Integration & Browser Test

**Step 1: Open in browser and verify**

- All sections render without borders
- Hero blur-in animation plays on load
- Scroll reveals trigger with blur-to-sharp
- Stat numbers magnify (scale up) as they reach viewport center
- Cost bars animate
- Counter animation works on stat numbers
- Nav dots track scroll position
- Responsive layout works at mobile widths
- No console errors

**Step 2: Fix any visual issues found during testing**

**Step 3: Final commit**

```bash
git add eldunari-pitch-v3.html
git commit -m "polish: final visual adjustments after browser testing"
```

---

## Summary

| Task | What | Focus |
|------|------|-------|
| 1 | CSS Variables & Typography | Foundation |
| 2 | Animation System | Replace generic with premium |
| 3 | Hero Section | Blur-in, scroll hint |
| 4 | Pitch Section | Editorial numbered steps |
| 5 | Use Cases | Asymmetric grid |
| 6 | Problem Section | Magnification stats |
| 7 | Outsourcing | Borderless comparison |
| 8 | AI Unlock | Refined cost bars |
| 9 | Data Control | Staggered layout |
| 10 | Cases & Founders | Polish |
| 11 | Nav & Responsive | Premium nav, mobile |
| 12 | JavaScript | Magnification engine |
| 13 | Integration Test | Browser verification |
