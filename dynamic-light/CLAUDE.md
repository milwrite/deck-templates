# CLAUDE.md — dynamic-light

This file is the primary reference for any agent working in this directory. Read it fully before editing any file.

---

## Skills

This template ships with two skills agents should use for slide work:

- **`skills/deck-editor/SKILL.md`** (in repo root) — the canonical protocol for editing slides: how to do line edits, replace slides, add/remove slides, fix aria attributes, and keep `SLIDES.md` in sync. Read this before making any edits to `index.html`.
- **Frontend design** — when making visual/styling changes to CSS (colors, layout, typography), apply design-system discipline: use existing design tokens from `css/styles.css` `:root`, do not introduce new color values without a token, maintain contrast ratios, and verify `prefers-reduced-motion` is respected in `css/animations.css`.

---

## Quick Start: Agent Checklist

You are filling in a slide deck template. Work through these steps in order:

1. **Read this file completely** before touching any code.
2. **Open `index.html`** — all slide content is marked with `[bracketed placeholders]`.
3. **Fill in deck metadata first** — slide 1 (title, subtitle, date, author) and the closing slide (slide 16). These frame the whole deck.
4. **Work slide by slide, top to bottom.** Replace every `[bracketed placeholder]` with real content. Delete slide blocks you don't need. Duplicate blocks to add more.
5. **When adding or removing slides**: renumber all `aria-label="Slide N: ..."` values on every affected slide, and update `aria-valuemax` on `#scrubber-container` to the new total.
6. **Images**: place files in `images/` using `slideN-letter.png` naming (e.g. `slide4-a.png`). If images aren't ready, leave the `<div class="screenshot-placeholder">` in place — it renders a visible placeholder box.
7. **Sync `SLIDES.md`** — update it to mirror the final slide-by-slide content of `index.html`.
8. **Commit** with a short lowercase message. No `Co-Authored-By` lines. No sign-off trailers.

Do not install packages, create new CSS files, or restructure the directory. All assets are already in place.

---

## Rules

- **Accessibility is required.** Every slide div must have `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`. Non-negotiable.
- **No em dashes** in slide content. Use commas, colons, or rewrite the sentence.
- **No Co-Authored-By lines** in commits.
- `SLIDES.md` must be synced before every commit.

---

## Project Overview

A template for a workshop or presentation series. Single-page HTML slide deck with a custom deck engine — no build system, no npm, no server required. Open `index.html` directly in a browser or push to any static host (GitHub Pages, Netlify, etc.).

---

## File Structure

```
dynamic-light/
├── index.html          ← source of truth for all slide content
├── SLIDES.md           ← markdown mirror of index.html — keep in sync
├── CLAUDE.md           ← this file
├── css/
│   ├── styles.css      ← design tokens, layout, all component styles
│   ├── responsive.css  ← breakpoint overrides (1024px, 768px, 480px)
│   └── animations.css  ← keyframes + prefers-reduced-motion override
├── js/
│   ├── tabs.js         ← tab component
│   ├── carousel.js     ← auto-advancing image carousel
│   ├── scrubber.js     ← scrubber timeline with ARIA slider support
│   └── deck-engine.js  ← core navigation — LOAD LAST, do not reorder
└── images/             ← all image assets go here
```

**JS load order matters.** The scripts at the bottom of `index.html` must stay in order: `tabs.js` → `carousel.js` → `scrubber.js` → `deck-engine.js`. Do not reorder or consolidate them.

---

## Slide Layouts

Each slide uses one layout class on its `.slide` div:

| Class | Description |
|---|---|
| `title-slide` | Opening/closing slide — dark, centered, full-screen |
| `layout-split` | Left `.content` panel + right `.stage` panel (50/50) |
| `layout-content` | Single column, light background — text-heavy slides |
| `layout-full-dark` | Centered, dark background — roadmaps, summaries |
| `layout-divider` | Section break — dark, large centered heading |
| `layout-grid` | Two-column card grid — comparisons, feature lists |

---

## Placeholder Reference

Every `[bracketed placeholder]` in `index.html` is intentional. Here is what each expects:

| Placeholder | Replace with |
|---|---|
| `[Workshop Title]` | The presentation title |
| `[Workshop Subtitle]` | One-line description or tagline |
| `[Date]` | Event date (e.g. "March 24, 2026") |
| `[Author Names]` | Presenter name(s) |
| `[Organization Logo]` | Remove div and add `<img>` if you have a logo; or delete the element |
| `[Series Title]` | Name of the workshop series (roadmap slide) |
| `[Session N Title]` | Individual session names in the roadmap |
| `[Section Label]` | Short uppercase label (e.g. "The Basics", "Part II") |
| `[Topic Title]` | Slide heading |
| `[Descriptive alt text...]` | Alt text for every image — required for accessibility |
| `images/placeholder-*.png` | Replace with real screenshot paths under `images/` |

---

## Progressive Reveal

Two mechanisms for building up content on a slide:

1. **Step reveal** — Add `class="step-hidden" data-step` to any element. Each forward advance (→ or Space) reveals the next hidden step. Steps reset when navigating away.
2. **Stream bullets** — `<ul class="stream-list">` items animate in with staggered timing (200ms + 250ms per item) automatically when the slide becomes active.

---

## Components

### Carousel
Place inside a `.stage` panel. `data-interval` is auto-advance in milliseconds:
```html
<div class="carousel" data-interval="8000">
  <div class="carousel-item active">
    <img src="images/slide4-a.png" alt="[Alt text]">
    <div class="carousel-caption"><strong>[Step Title]</strong><br>[Caption]</div>
  </div>
  <div class="carousel-item">
    <img src="images/slide4-b.png" alt="[Alt text]">
    <div class="carousel-caption"><strong>[Step Title]</strong><br>[Caption]</div>
  </div>
  <div class="carousel-dots"></div>
</div>
```

### Copyable Exercise Template
Right panel in exercise slides. The `id` must match what `copyTemplate()` targets:
```html
<div class="template-box">
  <pre id="template-1"><code>[FIELD]: _______________</code></pre>
  <button onclick="copyTemplate('template-1')" class="copy-btn">Copy to Clipboard</button>
</div>
```

### Screenshot Placeholder (when image not yet available)
```html
<div class="screenshot-placeholder">
  <div class="placeholder-icon" aria-hidden="true">🖼</div>
  <div class="placeholder-text">[Describe what screenshot goes here]</div>
</div>
```

### Tip Box
```html
<div class="tip-box">
  <p><strong>Note:</strong> [Your callout text here.]</p>
</div>
```

### Prompt Quality Progression
```html
<div class="prompt-block prompt-bad"><span class="prompt-label">Weak</span>"[weak example]"</div>
<div class="prompt-block prompt-mid step-hidden" data-step><span class="prompt-label">Getting Warmer</span>"[mid example]"</div>
<div class="prompt-block prompt-good step-hidden" data-step><span class="prompt-label">Strong</span>"[strong example]"</div>
```

---

## Adding a Slide

1. Copy the nearest slide block of the same layout type from `index.html`
2. Insert it at the right position
3. Update `aria-label="Slide N: [Title]"` on this slide
4. Renumber `aria-label` on every slide that comes after it
5. Update `aria-valuemax` on `#scrubber-container` to the new total slide count
6. Add content, replacing all `[bracketed placeholders]`
7. Sync `SLIDES.md`
8. Commit

## Removing a Slide

1. Delete the full `<div class="slide ...">...</div>` block
2. Renumber all downstream `aria-label` values
3. Update `aria-valuemax` on `#scrubber-container`
4. Sync `SLIDES.md`
5. Commit

---

## Design Tokens

All color and typography tokens are in `css/styles.css` under `:root`. Use token references, not raw hex values:

| Token | Role |
|---|---|
| `--bg-light` | Light slide background |
| `--bg-panel` | Stage/right-panel background |
| `--dark-bg` | Dark slide background |
| `--dark-bg-lighter` | Lighter dark (cards on dark slides) |
| `--accent-cyan` | Primary accent: bullet dots, labels, progress bar |
| `--accent-blue` | Secondary accent: cards, roadmap borders |
| `--text-dark` | Body text on light backgrounds |
| `--text-muted` | Secondary/subdued text |
| `--light-text` | Text on dark backgrounds |
| `--code-bg` | Background for prompt/code blocks |

Typography: `Fraunces` (headings), `DM Sans` (body), `JetBrains Mono` (code blocks) — loaded from Google Fonts CDN in `index.html` head.

---

## Accessibility Checklist

Before committing:
- [ ] Every `.slide` div has `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`
- [ ] All slide numbers are sequential with no gaps
- [ ] `aria-valuemax` on `#scrubber-container` matches the total slide count
- [ ] Every `<img>` has a descriptive `alt` attribute (or `alt=""` for purely decorative images)
- [ ] Decorative emoji have `aria-hidden="true"`
- [ ] No content relies on color alone to convey meaning

---

## Commit Conventions

- Short, lowercase message (e.g. `add slide 5 on prompt design`)
- No `Co-Authored-By` lines
- No sign-off trailers (`Signed-off-by:`)
- Sync `SLIDES.md` before committing
