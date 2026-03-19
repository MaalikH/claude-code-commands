---
name: modern-frontend
slug: modern-frontend
version: 1.0.0
description: Build premium, restrained frontend interfaces inspired by Resend, Linear, Harvey.ai, Nudge, Realm, Cycle, and Duna. Enforces a curated design system with specific palettes, fonts, motion limits, and Bootstrap 5 integration. Replaces generic AI aesthetics with intentional, bespoke design.
license: MIT
---

This skill enforces a premium, restrained design system for frontend work. Every decision is opinionated and concrete — no vague guidance, no "pick an extreme." The output should look like it belongs alongside Resend, Linear, Harvey.ai, Nudge, Realm, Cycle, or Duna.

The user provides frontend requirements. Apply this entire system automatically. Do not ask which mood or palette — infer from context, or default to Dark Technical.

---

## 1. Core Philosophy

Four principles govern every decision:

1. **Intentional Restraint** — Every element earns its place. Remove before adding. White space is a feature.
2. **Singular Aesthetic** — One mood, committed fully. No hedging between styles.
3. **Bespoke Character** — No element should look like it came from a template. Typography, spacing, and color must feel hand-picked.
4. **Premium Simplicity** — Complexity hidden behind effortless surfaces. Like a Porsche interior — few controls, each one perfect.

---

## 2. Decision Framework — 4 Mood Directions

Pick ONE based on context. Each maps to a palette, font pairing, and reference.

### Dark Technical
- **Vibe**: Developer tools, command lines, precision instruments
- **References**: Linear, Resend, Vercel, Terminal-X (terminal-x.ai), Speakeasy (speakeasy.com)
- **Palette**: Obsidian (see Section 4)
- **Fonts**: Satoshi + JetBrains Mono
- **Accent placement**: Status indicators, interactive elements, code highlights
- **Best for**: Dev tools, dashboards, SaaS products, API docs

### Warm Editorial
- **Vibe**: Magazine layouts, literary polish, considered storytelling
- **References**: Harvey.ai, Duna (duna.com), Relace (relace.ai), Panxo (panxo.com)
- **Palette**: Parchment (see Section 4)
- **Fonts**: Instrument Serif + General Sans
- **Accent placement**: Pull quotes, section markers, hover states
- **Best for**: Legal tech, content platforms, editorial sites, portfolios

### Clean Clinical
- **Vibe**: Medical precision, laboratory clarity, absolute order
- **References**: Nudge (getnudge.info), Realm (withrealm.com), Exactly.ai
- **Palette**: Bone (see Section 4)
- **Fonts**: Switzer + Cabinet Grotesk
- **Accent placement**: CTAs only, data visualization accents, form focus states
- **Best for**: Health tech, fintech, enterprise SaaS, analytics

### Expressive Product
- **Vibe**: Confident product showcase, controlled energy, brand-forward
- **References**: Cycle, Nudge
- **Palette**: Midnight Warm (see Section 4)
- **Fonts**: Clash Display + General Sans
- **Accent placement**: Hero elements, feature highlights, pricing CTAs
- **Best for**: Product marketing, landing pages, startup sites, feature launches

### Liquid Glass
- **Vibe**: Apple-native translucency, frosted layers, system-level polish
- **References**: Apple iOS Liquid Glass, @heyeaslo's Easlo, Today App
- **Palette**: Frost (see Section 4)
- **Fonts**: SF Pro Display + SF Pro Text (system), or Satoshi + Switzer as web fallback
- **Accent placement**: Tinted glass sheets, progress indicators, active tab highlights
- **Best for**: iOS native apps, Apple ecosystem products, health/wellness apps, productivity tools
- **Key traits**: `backdrop-filter: blur()` layering, translucent sheets over content, SF Symbols for iconography, light+dark mode parity, rounded corners (20px+), system-level spacing

### Dither Punk
- **Vibe**: Retro-digital grain, pixel art meets modern layout, lo-fi textures with hi-fi structure
- **References**: TukiFromKL, mail0, early-internet aesthetics reimagined
- **Palette**: Cathode (see Section 4)
- **Fonts**: Cabinet Grotesk + JetBrains Mono (or any monospace for body)
- **Accent placement**: Dithered hero imagery, dot-matrix headings, halftone section dividers
- **Best for**: Brand-forward sites, experimental UI, auth/login screens, creative studios, dev tools with personality
- **Key traits**: Halftone/dither image treatment (CSS or SVG filter), dot-matrix or pixel typography for display, limited 2-3 color palette, high contrast, deliberate lo-fi grain as texture

---

## 3. Typography

### Curated Font List — USE ONLY THESE

**From Fontshare (https://api.fontshare.com/v2/css):**
| Font | Weight Range | Use |
|------|-------------|-----|
| Satoshi | 400, 500, 700 | Body + UI (Dark Technical) |
| General Sans | 400, 500, 600 | Body + UI (Warm Editorial, Expressive Product) |
| Cabinet Grotesk | 500, 700, 800 | Headlines (Clean Clinical) |
| Clash Display | 500, 600, 700 | Headlines (Expressive Product) |
| Switzer | 400, 500 | Body (Clean Clinical) |

**From Google Fonts:**
| Font | Weight Range | Use |
|------|-------------|-----|
| Instrument Serif | 400 | Headlines (Warm Editorial) |
| Playfair Display | 400, 700 | Headlines (alternate Warm Editorial) |
| Fraunces | 400, 700 | Display text, feature callouts |
| JetBrains Mono | 400, 500 | Code blocks, monospace UI |

### Loading Fonts

Fontshare — add to `<head>`:
```html
<link href="https://api.fontshare.com/v2/css?f[]=satoshi@400,500,700&f[]=general-sans@400,500,600&f[]=cabinet-grotesk@500,700,800&f[]=clash-display@500,600,700&f[]=switzer@400,500&display=swap" rel="stylesheet">
```
Only include the fonts you actually use. Strip unused families from the URL.

Google Fonts — add to `<head>`:
```html
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### Typography Scale

```css
:root {
  --font-heading: 'Clash Display', sans-serif;  /* swap per mood */
  --font-body: 'General Sans', sans-serif;       /* swap per mood */
  --font-mono: 'JetBrains Mono', monospace;

  --text-xs: clamp(0.7rem, 0.65rem + 0.25vw, 0.8rem);
  --text-sm: clamp(0.8rem, 0.75rem + 0.3vw, 0.9rem);
  --text-base: clamp(0.95rem, 0.9rem + 0.3vw, 1.05rem);
  --text-lg: clamp(1.15rem, 1rem + 0.5vw, 1.35rem);
  --text-xl: clamp(1.4rem, 1.1rem + 1vw, 1.8rem);
  --text-2xl: clamp(1.8rem, 1.3rem + 1.5vw, 2.6rem);
  --text-3xl: clamp(2.2rem, 1.5rem + 2.5vw, 3.5rem);
  --text-display: clamp(2.8rem, 1.8rem + 3.5vw, 5rem);
}

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: 1.6;
  letter-spacing: -0.01em;
  -webkit-font-smoothing: antialiased;
}

h1, h2, h3 {
  font-family: var(--font-heading);
  line-height: 1.15;
  letter-spacing: -0.025em;
}
```

### BANNED FONTS — Never use these
`Inter, Roboto, Poppins, Montserrat, Open Sans, Lato, Nunito, Raleway, Space Grotesk, DM Sans, Work Sans, Source Sans Pro, Arial, Helvetica, system-ui (as visible font)`

If you catch yourself reaching for any of these, stop and pick from the curated list above.

---

## 4. Color Palettes — CSS Variable Presets

Each palette is copy-paste ready. Use ONE palette per project.

### Obsidian (Dark Technical)
```css
:root {
  --bg-primary: #0a0a0b;
  --bg-secondary: #111113;
  --bg-elevated: #18181b;
  --border: #27272a;
  --border-subtle: #1e1e21;
  --text-primary: #fafafa;
  --text-secondary: #a1a1aa;
  --text-muted: #52525b;
  --accent: #3b82f6;
  --accent-hover: #60a5fa;
  --accent-subtle: rgba(59, 130, 246, 0.12);
}
```

### Parchment (Warm Editorial)
```css
:root {
  --bg-primary: #f8f5f0;
  --bg-secondary: #f0ebe4;
  --bg-elevated: #ffffff;
  --border: #d6cfc5;
  --border-subtle: #e8e2d9;
  --text-primary: #1a1714;
  --text-secondary: #5c564e;
  --text-muted: #8a847b;
  --accent: #b45309;
  --accent-hover: #d97706;
  --accent-subtle: rgba(180, 83, 9, 0.08);
}
```

### Bone (Clean Clinical)
```css
:root {
  --bg-primary: #fafaf9;
  --bg-secondary: #f5f5f4;
  --bg-elevated: #ffffff;
  --border: #e7e5e4;
  --border-subtle: #f0eeec;
  --text-primary: #0c0a09;
  --text-secondary: #57534e;
  --text-muted: #a8a29e;
  --accent: #0d9488;
  --accent-hover: #14b8a6;
  --accent-subtle: rgba(13, 148, 136, 0.08);
}
```

### Midnight Warm (Expressive Product)
```css
:root {
  --bg-primary: #0f0e17;
  --bg-secondary: #171626;
  --bg-elevated: #1f1d36;
  --border: #2e2b50;
  --border-subtle: #232144;
  --text-primary: #fffffe;
  --text-secondary: #a7a9be;
  --text-muted: #5e5f79;
  --accent: #ff6e6c;
  --accent-hover: #ff8f8d;
  --accent-subtle: rgba(255, 110, 108, 0.12);
}
```

### Frost (Liquid Glass)
```css
:root {
  --bg-primary: #f2f2f7;
  --bg-secondary: rgba(255, 255, 255, 0.72);
  --bg-elevated: rgba(255, 255, 255, 0.85);
  --border: rgba(0, 0, 0, 0.08);
  --border-subtle: rgba(0, 0, 0, 0.04);
  --text-primary: #1c1c1e;
  --text-secondary: #3c3c43;
  --text-muted: #8e8e93;
  --accent: #007aff;
  --accent-hover: #0a84ff;
  --accent-subtle: rgba(0, 122, 255, 0.10);
  --glass-bg: rgba(255, 255, 255, 0.65);
  --glass-blur: 20px;
  --glass-border: rgba(255, 255, 255, 0.3);
}

/* Dark mode variant */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #000000;
    --bg-secondary: rgba(28, 28, 30, 0.72);
    --bg-elevated: rgba(44, 44, 46, 0.85);
    --border: rgba(255, 255, 255, 0.08);
    --border-subtle: rgba(255, 255, 255, 0.04);
    --text-primary: #ffffff;
    --text-secondary: #ebebf5;
    --text-muted: #636366;
    --glass-bg: rgba(44, 44, 46, 0.65);
    --glass-border: rgba(255, 255, 255, 0.1);
  }
}
```

### Cathode (Dither Punk)
```css
:root {
  --bg-primary: #0d0d0d;
  --bg-secondary: #141414;
  --bg-elevated: #1a1a1a;
  --border: #2a2a2a;
  --border-subtle: #1f1f1f;
  --text-primary: #e8e8e8;
  --text-secondary: #888888;
  --text-muted: #555555;
  --accent: #ff6b35;
  --accent-hover: #ff8555;
  --accent-subtle: rgba(255, 107, 53, 0.12);
  --dither-opacity: 0.15;
}
```

### Color Rules
- **Accent in max 3 places per viewport**. If accent is everywhere, it's nowhere.
- Use `--text-secondary` for descriptions, `--text-muted` for metadata/timestamps.
- `--border-subtle` for dividers, `--border` for interactive element borders.
- `--bg-elevated` for cards and modals — creates depth without shadow abuse.

---

## 5. Layout — Bootstrap 5 Integration

### Bootstrap CDN — add to `<head>`:
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
```
Add Bootstrap JS before `</body>` only if you need interactive components (dropdowns, modals, collapse):
```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

### How to Use Bootstrap 5 Here

Bootstrap handles **structural layout only**: `container`, `row`, `col-*`, `g-*` gutters, responsive breakpoints, and utility classes for spacing (`mt-*`, `py-*`, etc.).

**Everything visual is custom**: colors, fonts, borders, shadows, backgrounds, and component styling override Bootstrap defaults via CSS custom properties. Never rely on Bootstrap's default look.

```css
/* Override Bootstrap's defaults */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: var(--font-body);
}

.container, .container-lg, .container-xl {
  max-width: 1120px; /* Tighter than Bootstrap's default 1140px */
}

.btn {
  font-family: var(--font-body);
  font-weight: 500;
  border-radius: 8px;
  transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Hero Patterns (3 options — pick one)

**A. Statement Hero** — Big type, minimal elements
```html
<section class="hero-statement">
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-lg-8 text-center">
        <p class="hero-label">Introducing Acme</p>
        <h1 class="hero-title">Build software<br>that ships faster</h1>
        <p class="hero-description">The modern toolkit for teams who care about craft.</p>
        <div class="hero-actions d-flex gap-3 justify-content-center">
          <a href="#" class="btn btn-primary-custom">Get started</a>
          <a href="#" class="btn btn-ghost">Learn more</a>
        </div>
      </div>
    </div>
  </div>
</section>
```
```css
.hero-statement {
  padding: clamp(6rem, 12vh, 10rem) 0 clamp(4rem, 8vh, 8rem);
}
.hero-label {
  font-size: var(--text-sm);
  font-weight: 500;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 1.5rem;
}
.hero-title {
  font-size: var(--text-display);
  margin-bottom: 1.5rem;
}
.hero-description {
  font-size: var(--text-lg);
  color: var(--text-secondary);
  max-width: 480px;
  margin: 0 auto 2.5rem;
}
```

**B. Split Hero** — Text left, visual right
```html
<section class="hero-split">
  <div class="container">
    <div class="row align-items-center g-5">
      <div class="col-lg-6">
        <h1 class="hero-title">...</h1>
        <p class="hero-description">...</p>
        <a href="#" class="btn btn-primary-custom">Get started</a>
      </div>
      <div class="col-lg-6">
        <div class="hero-visual"><!-- Screenshot, graphic, or code block --></div>
      </div>
    </div>
  </div>
</section>
```

**C. Compact Hero** — Left-aligned, editorial feel
```html
<section class="hero-compact">
  <div class="container">
    <div class="row">
      <div class="col-lg-7">
        <h1 class="hero-title">...</h1>
      </div>
    </div>
    <div class="row mt-4">
      <div class="col-lg-5">
        <p class="hero-description">...</p>
        <a href="#" class="btn btn-primary-custom">Get started</a>
      </div>
    </div>
  </div>
</section>
```

### Spacing System
```css
:root {
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --space-xl: 2.5rem;
  --space-2xl: 4rem;
  --space-3xl: 6rem;
  --section-gap: clamp(4rem, 8vh, 8rem);
}

section { padding: var(--section-gap) 0; }
section + section { border-top: 1px solid var(--border-subtle); }
```

---

## 6. Visual Texture

### Grain Overlay (SVG noise — 3-5% opacity)
```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 9999;
  pointer-events: none;
  opacity: 0.035;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  background-repeat: repeat;
  background-size: 256px 256px;
}
```

### Radial Glow (accent color, hero sections only)
```css
.hero-glow {
  position: absolute;
  width: 600px;
  height: 600px;
  border-radius: 50%;
  background: radial-gradient(circle, var(--accent-subtle) 0%, transparent 70%);
  filter: blur(80px);
  pointer-events: none;
  z-index: 0;
}
```

### Shadow Scale (use consistently)
```css
:root {
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.08);
  --shadow-lg: 0 12px 40px rgba(0,0,0,0.12);
  --shadow-glow: 0 0 20px var(--accent-subtle);
}
```

### Glass Effect (1 per page max — use sparingly, except Liquid Glass mood)
```css
.glass {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid var(--border-subtle);
  border-radius: 12px;
}
```

### Liquid Glass Layers (Liquid Glass mood only)
Glass is the primary design language here — not limited to 1 per page.
```css
.glass-sheet {
  background: var(--glass-bg);
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  border: 1px solid var(--glass-border);
  border-radius: 20px;
  padding: var(--space-xl);
}

.glass-sheet-elevated {
  background: var(--bg-elevated);
  backdrop-filter: blur(40px);
  -webkit-backdrop-filter: blur(40px);
  border: 1px solid var(--glass-border);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.06);
}
```

### Dither Texture (Dither Punk mood only)
```css
/* SVG halftone/dither overlay */
.dither-overlay {
  position: relative;
}
.dither-overlay::after {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: var(--dither-opacity, 0.15);
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 4 4' xmlns='http://www.w3.org/2000/svg'%3E%3Crect x='0' y='0' width='1' height='1' fill='%23fff'/%3E%3Crect x='2' y='2' width='1' height='1' fill='%23fff'/%3E%3C/svg%3E");
  background-size: 4px 4px;
  mix-blend-mode: overlay;
}

/* Dithered image treatment */
.dither-image {
  filter: contrast(1.2) grayscale(0.8);
  image-rendering: pixelated;
}
```

---

## 7. Motion

### Core Easing
```css
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
}
```

### Entrance Animation (fadeUp)
```css
@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(12px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-up {
  opacity: 0;
  animation: fadeUp 0.6s var(--ease-out) forwards;
}

/* Stagger children */
.stagger > * { opacity: 0; }
.stagger.visible > *:nth-child(1) { animation: fadeUp 0.5s var(--ease-out) 0.0s forwards; }
.stagger.visible > *:nth-child(2) { animation: fadeUp 0.5s var(--ease-out) 0.08s forwards; }
.stagger.visible > *:nth-child(3) { animation: fadeUp 0.5s var(--ease-out) 0.16s forwards; }
.stagger.visible > *:nth-child(4) { animation: fadeUp 0.5s var(--ease-out) 0.24s forwards; }
.stagger.visible > *:nth-child(5) { animation: fadeUp 0.5s var(--ease-out) 0.32s forwards; }
.stagger.visible > *:nth-child(6) { animation: fadeUp 0.5s var(--ease-out) 0.40s forwards; }
```

### Scroll Reveal (Intersection Observer)
```html
<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });

document.querySelectorAll('.stagger, .fade-up-scroll').forEach(el => observer.observe(el));
</script>
```

### Hard Motion Limits
| Property | Max Value | Reason |
|----------|-----------|--------|
| Hover transform | `translateY(-4px)` | Anything more feels janky |
| Transition duration | `0.8s` | Longer feels sluggish |
| Animation delay (stagger) | `0.08s` increments | Keeps rhythm tight |
| Scale on hover | `1.02` max | Subtle, not bouncy |
| Blur on glass | `12px` max | More kills readability |

### Reduced Motion
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 8. Buttons & Cards

### Buttons
```css
.btn-primary-custom {
  background: var(--text-primary);
  color: var(--bg-primary);
  border: none;
  padding: 0.65rem 1.5rem;
  font-size: var(--text-sm);
  font-weight: 500;
  border-radius: 8px;
  cursor: pointer;
  transition: opacity 0.2s var(--ease-out);
}
.btn-primary-custom:hover {
  opacity: 0.85;
}

.btn-ghost {
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid var(--border);
  padding: 0.65rem 1.5rem;
  font-size: var(--text-sm);
  font-weight: 500;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s var(--ease-out);
}
.btn-ghost:hover {
  color: var(--text-primary);
  border-color: var(--text-muted);
}
```

**Button rules:**
- Primary button = inverted colors (light text on dark bg or dark text on light bg)
- No gradient buttons. Ever.
- Border-radius: `8px` (not fully rounded pills unless explicitly requested)
- Hover = opacity shift or border change, not color swap
- Max 2 button styles per page (primary + ghost)

### Cards
```css
.card-custom {
  background: var(--bg-elevated);
  border: 1px solid var(--border-subtle);
  border-radius: 12px;
  padding: var(--space-xl);
  transition: border-color 0.2s var(--ease-out);
}
.card-custom:hover {
  border-color: var(--border);
}
```

**Card rules:**
- 1px border over box-shadow for definition (use shadows sparingly for depth, not decoration)
- No colored card backgrounds — use `--bg-elevated` only
- Hover state = subtle border color shift, optional `translateY(-2px)`
- Consistent border-radius across all cards (`12px`)

---

## 9. Navigation

```css
.navbar-custom {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 100;
  padding: 0.75rem 0;
  border-bottom: 1px solid var(--border-subtle);
  background: var(--bg-primary);
  /* Optional: slight transparency */
  /* background: rgba(var(--bg-primary-rgb), 0.85); */
  /* backdrop-filter: blur(12px); */
}

.nav-link-custom {
  color: var(--text-secondary);
  font-size: var(--text-sm);
  font-weight: 500;
  text-decoration: none;
  transition: color 0.15s var(--ease-out);
}
.nav-link-custom:hover {
  color: var(--text-primary);
}
```

**Nav rules:**
- Fixed or sticky top, minimal height
- Logo left, links center or right — nothing else
- No mega menus, no icons in nav links (unless icon-only mobile)
- Active state = `color: var(--text-primary)` + optional subtle underline

---

## 10. Anti-Patterns — BANNED

### Banned Hex Values
These colors scream "AI generated" or "Bootstrap default." Never use them:
`#6366f1, #8b5cf6, #a855f7` (the purple gradient trio),
`#7c3aed` (Tailwind violet-600),
`#6d28d9` (Tailwind violet-700),
`#f97316` (generic orange),
`#10b981` (generic green),
`#667eea, #764ba2` (the cliche gradient pair)

### Banned Layouts
- Full-width hero with centered text + screenshot below + 3-column feature grid = the AI trifecta. Avoid this exact combination.
- Cards with icon circles above headlines in a 3-column grid
- Alternating left-right feature sections with device mockups

### Banned Effects
- Parallax scrolling (performance disaster, feels 2015)
- Text gradient on everything (one per page max, if earned)
- Bouncing animations or `ease-in-out` spring effects
- Drop shadows on everything — pick borders OR shadows, not both on the same element
- Rotating/spinning loaders or icons
- Background video (loading nightmare)

### Banned Copy Patterns
- "Revolutionize your workflow"
- "Built for teams that ship"
- "The modern platform for..."
- "Supercharge your..."
- "10x your productivity"
- Any headline ending in "— in minutes"
- "Join 10,000+ companies" (unless true and verified)

---

## 11. Implementation Checklist

Every project must pass ALL 18 items. Binary pass/fail.

### Typography (4 items)
- [ ] All fonts from curated list (Section 3) — no banned fonts
- [ ] Heading font differs from body font (proper pairing)
- [ ] Font sizes use `clamp()` scale — no hardcoded px for text
- [ ] `letter-spacing` applied to headings (negative) and labels (positive)

### Color (4 items)
- [ ] Using one of the 4 palette presets — no custom hex outside the palette
- [ ] Accent color appears in 3 or fewer places per viewport
- [ ] Dark backgrounds use `--bg-primary` (not `#000` or `black`)
- [ ] No banned hex values appear anywhere in the CSS

### Layout (3 items)
- [ ] Bootstrap 5 CDN loaded and grid used for structural layout
- [ ] Max content width between 1000px and 1200px
- [ ] Section spacing uses `--section-gap` with `clamp()`

### Motion (3 items)
- [ ] `prefers-reduced-motion` media query present
- [ ] No transition/animation exceeds 0.8s
- [ ] Hover transforms max `translateY(-4px)` or `scale(1.02)`

### Visual Quality (4 items)
- [ ] Grain overlay applied (body::before with SVG noise)
- [ ] No gradient buttons anywhere
- [ ] Cards use 1px borders, not box-shadows for definition
- [ ] Glass effect used max 1 time (or not at all)

---

## 12. Accessibility

Non-negotiable requirements:

- **Contrast**: Text meets WCAG AA (4.5:1 body, 3:1 large text). Use the palette as provided — values are pre-checked.
- **Focus states**: All interactive elements have visible focus rings:
```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```
- **Keyboard navigation**: Tab order follows visual order. Skip links if navigation is complex.
- **Semantic HTML**: Use `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`. Headings in order (`h1` > `h2` > `h3`).
- **Reduced motion**: Already covered in Section 7.
- **Alt text**: Every `<img>` has descriptive alt text. Decorative images get `alt=""` and `aria-hidden="true"`.
- **Touch targets**: Min 44x44px for buttons and links on mobile.

---

## 13. Responsive Behavior

Use Bootstrap 5 breakpoints. Custom styles layer on top:

```css
/* Mobile-first overrides */
@media (max-width: 767.98px) {
  .hero-title { font-size: var(--text-2xl); }
  .hero-description { font-size: var(--text-base); }
  section { padding: clamp(2.5rem, 5vh, 4rem) 0; }
}

@media (max-width: 575.98px) {
  .hero-actions { flex-direction: column; }
  .hero-actions .btn { width: 100%; }
}
```

- Stack columns below `lg` breakpoint (Bootstrap handles this)
- Reduce hero typography at mobile
- Full-width buttons at `xs`
- Hide non-essential visual flourishes (glows, grain) on mobile for performance

---

## 14. Complete HTML Boilerplate

Starting template for any new page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>

  <!-- Bootstrap 5 -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

  <!-- Fonts (only include what you use) -->
  <link href="https://api.fontshare.com/v2/css?f[]=satoshi@400,500,700&display=swap" rel="stylesheet">

  <!-- Custom Styles -->
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header class="navbar-custom">
    <div class="container d-flex align-items-center justify-content-between">
      <a href="/" class="nav-logo">Logo</a>
      <nav class="d-flex gap-4">
        <a href="#" class="nav-link-custom">Features</a>
        <a href="#" class="nav-link-custom">Pricing</a>
        <a href="#" class="nav-link-custom">Docs</a>
      </nav>
      <a href="#" class="btn btn-primary-custom btn-sm">Get started</a>
    </div>
  </header>

  <main style="padding-top: 4rem;">
    <!-- Hero section -->
    <!-- Content sections -->
  </main>

  <footer>
    <div class="container py-5">
      <div class="row">
        <!-- Footer content -->
      </div>
    </div>
  </footer>

  <!-- Bootstrap JS (only if needed for interactive components) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

  <!-- Scroll reveal -->
  <script>
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });
    document.querySelectorAll('.stagger, .fade-up-scroll').forEach(el => observer.observe(el));
  </script>
</body>
</html>
```

---

## 15. Maalik's Design Preferences

When Maalik says "make it modern," prioritize these patterns (extracted from his review of all 13 inspiration sites):

### Layout Priorities
1. **Section-based layouts over conventional heroes** — The page should be a series of strong sections, not just a hero + content dump. Reference: Exactly.ai, Panxo, Relace, Nudge, Duna.
2. **Big visuals** — Oversized cards, large product screenshots in Mac displays, widescreen graphs. Nothing timid. Reference: Exactly.ai, Panxo.
3. **Metrics as visual anchors** — Large numbers (10M, 400K, etc.) as section focal points. Reference: Panxo, Duna.
4. **Flat design with real screenshots** — Product UI embedded directly, not illustrations or mockups. Reference: Linear.
5. **FAQ accordions** — For any page with frequently asked questions. Reference: Relace.

### Scroll Interactions (Highly Valued)
1. **Horizontal card scrolls** — Large section cards that scroll horizontally. Reference: Exactly.ai.
2. **Vertical section transitions** — Dark-to-light (or vice versa) scroll-triggered section changes. Reference: Terminal-X.
3. **Side-section scroll** — Content scrolls while sidebar sections remain fixed/sticky. Reference: Realm's "unfair advantage" section.
4. **Parallax** — Despite being in the banned list generally, Maalik specifically likes parallax when done purposefully (e.g., Vide Infra's second section). Use sparingly and only when Maalik requests it.

### Dark Site Preferences
- **Preferred dark background**: Linear's style (#0a0a0b range) — use the Obsidian palette.
- Terminal-X is a close second (dark/white section alternation).
- Resend-style dark is good but not the default choice.

### Color/Background Preferences
- **Warm backgrounds**: Relace's yellow/note-taking tone, Speakeasy's slate gray.
- **Color combos**: Gray + red accent (Nemanja Kocovic style) is appealing.
- **Basic layouts work** when consistent — Realm, Nudge, Duna prove you don't need complexity to look modern.

### Auth Page Layout — Half-Split (MANDATORY)
All auth pages (login, signup, forgot password, reset password, onboarding/questionnaire) **MUST** use the half-split layout pattern. No centered cards. No single-column forms.

**Reference**: Payper (payper.so) — dark, clean, 50/50 split. Left = form/actions, Right = brand panel with imagery + tagline.

**Structure**:
```html
<div class="auth-split">
  <!-- Left: Form panel -->
  <div class="auth-split-form">
    <div class="auth-form-inner">
      <div class="auth-logo"><!-- Logo/icon --></div>
      <h1 class="auth-heading">Get started with Brand</h1>
      <p class="auth-subtext">Short value prop or instruction.</p>
      <!-- Form fields or action buttons -->
      <form>...</form>
      <p class="auth-footer-link">Already have an account? <a href="/login">Log in</a></p>
    </div>
  </div>

  <!-- Right: Brand panel -->
  <div class="auth-split-brand">
    <div class="auth-brand-content">
      <!-- Brand image, 3D render, product screenshot, or illustration -->
      <div class="auth-brand-visual">
        <img src="..." alt="" />
      </div>
      <h2 class="auth-brand-tagline">Connect With Brands</h2>
      <p class="auth-brand-description">Short brand message or benefit statement.</p>
    </div>
  </div>
</div>
```

**CSS**:
```css
.auth-split {
  display: flex;
  min-height: 100vh;
}

.auth-split-form {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  background: var(--bg-primary);
}

.auth-form-inner {
  width: 100%;
  max-width: 400px;
}

.auth-split-brand {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  background: var(--bg-secondary);
  border-left: 1px solid var(--border-subtle);
}

.auth-brand-content {
  text-align: center;
  max-width: 420px;
}

.auth-brand-visual {
  margin-bottom: 2rem;
}

.auth-brand-visual img {
  max-width: 280px;
  height: auto;
}

.auth-brand-tagline {
  font-family: var(--font-heading);
  font-size: var(--text-xl);
  margin-bottom: 0.75rem;
}

.auth-brand-description {
  font-size: var(--text-sm);
  color: var(--text-secondary);
}

.auth-logo {
  margin-bottom: 2rem;
}

.auth-heading {
  font-family: var(--font-heading);
  font-size: var(--text-2xl);
  margin-bottom: 0.5rem;
}

.auth-subtext {
  color: var(--text-secondary);
  font-size: var(--text-sm);
  margin-bottom: 2rem;
}

.auth-footer-link {
  text-align: center;
  margin-top: 1.5rem;
  font-size: var(--text-sm);
  color: var(--text-muted);
}

.auth-footer-link a {
  color: var(--text-primary);
  font-weight: 500;
  text-decoration: none;
}

/* Responsive: stack on mobile */
@media (max-width: 991.98px) {
  .auth-split {
    flex-direction: column;
  }
  .auth-split-brand {
    display: none; /* Hide brand panel on mobile — form is priority */
  }
  .auth-split-form {
    min-height: 100vh;
  }
}
```

**Rules**:
- Left panel: white/dark bg (`--bg-primary`), vertically centered form, max-width 400px
- Right panel: slightly different shade (`--bg-secondary`), brand imagery + tagline centered
- On mobile (<992px): hide the brand panel entirely, form takes full viewport
- Buttons are full-width within the form panel
- Same layout for login, signup, forgot password, reset password, AND onboarding/questionnaire steps
- Brand panel can rotate content (different taglines/visuals per page) or stay static

### Reference: Full site-by-site preferences
See `~/.claude/projects/-Users-maalikahmadtech-Dev/memory/design-inspiration.md` for detailed per-site analysis and Maalik's specific notes on each.

---

## Quick Reference

| Decision | Answer |
|----------|--------|
| Which CSS framework? | Bootstrap 5 for grid, custom CSS for design |
| Which fonts? | See Section 3 curated list only |
| Which colors? | One of 6 palettes from Section 4 |
| How much animation? | fadeUp + stagger + hover. Max 0.8s. |
| Gradients? | Background glows yes, button gradients no |
| Glass/blur? | 1 per page max |
| Shadows or borders? | Borders for cards. Shadows for elevation only. |
| Purple? | No. |
