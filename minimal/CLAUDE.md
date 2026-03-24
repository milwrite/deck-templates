# CLAUDE.md — minimal

This file is the primary reference for any agent working in this directory. Read it fully before editing any file.

---

## Quick Start: Agent Checklist

You are filling in a slide deck template. Work through these steps in order:

1. **Read this file completely** before touching any code.
2. **Open `index.html`** — this is the only file you need to edit. All CSS and JS are inline. All slide content is marked with `[bracketed placeholders]`.
3. **Fill in deck metadata first** — slide 1 (event label, title, subtitle, date, presenter credit). This frames the whole deck.
4. **Work slide by slide, top to bottom.** Replace every `[bracketed placeholder]` with real content. Delete slide sections you don't need. Copy a section block to add more.
5. **When adding or removing slides**: update `aria-label="Slide N: ..."` on every affected section, renumber all downstream slides, and update `max="N"` on `#slide-scrubber` in the footer.
6. **Sync `SLIDES.md`** — update it to mirror the final content of `index.html`.
7. **Commit** with a short lowercase message. No `Co-Authored-By` lines. No sign-off trailers.

Do not create external CSS or JS files. Everything is intentionally self-contained in `index.html`.

---

## Rules

- **Accessibility is required.** Every `<section class="slide">` must have `aria-label="Slide N: [Title]"` and `tabindex="-1"`. Non-negotiable.
- **No em dashes** in slide content. Use commas, colons, or rewrite.
- **No Co-Authored-By lines** in commits.
- `SLIDES.md` must be synced before every commit.

---

## Project Overview

A self-contained dark two-panel slide deck. All CSS and JS live inline in `index.html` — no separate files, no build step, no dependencies. Open directly in a browser or push to any static host.

Best for compact presentations, roundtables, talks with sequential step reveals, or any context where a single portable file is preferable.

---

## File Structure

```
minimal/
├── index.html    ← the only file — all slides, CSS (<style>), and JS (<script>) are here
├── SLIDES.md     ← markdown mirror of slide content — keep in sync
└── CLAUDE.md     ← this file
```

---

## Slide Structure

Each slide is a `<section class="slide">` inside `<main>`. The base pattern:

```html
<section class="slide" data-slide="unique-id" aria-label="Slide N: [Title]" tabindex="-1">
  <div class="content">
    <!-- label, h1, h2, date, body text on the left -->
  </div>
  <div class="stage" aria-hidden="true">
    <!-- step-grid, stageCompare, res-wrap, or iframe on the right -->
  </div>
</section>
```

On tablet/desktop, `.content` and `.stage` sit side by side (44% / 52%). On mobile they stack vertically.

**Two exceptions:**
- **Title slide**: `data-slide="title"` — special CSS makes the stage full-height. Use `.stage-fill > iframe` or `canvas` for a live embed, or leave `.stage` empty.
- **Section break**: add class `slide-break` — no `.stage` needed.

---

## Slide Types

| Pattern | Stage content | When to use |
|---|---|---|
| Step grid | `.step-grid` > `.step-row.frag` | Sequential numbered reveals, process steps, key points |
| Compare | `.stageCompare` > two `.col` divs | Two-way contrasts, before/after, frameworks |
| Resources | `.res-wrap` > `.res-group` > `.res-list` | Links, citations, further reading |
| Section break | (no stage, `slide-break` class) | Transitioning between major sections |
| Embed | `.stage-fill` > `iframe` or `canvas` | Live visualization, demo, interactive tool |
| Closing | `.step-grid` with discussion prompts as `.frag` rows | Q&A, discussion questions, next steps |

---

## Fragment Reveal

Add `class="frag"` to any element inside `.stage`. On each Space/→ keypress (or next button click):
- The next hidden `.frag` element gets class `visible` added (opacity 0→1, slight translateY)
- Once all fragments on the current slide are visible, the next advance moves to the next slide
- Navigating back to a slide reveals all its fragments at once

`.frag` works on any element — a `.step-row`, a `.col`, an `<li>`, a block of text.

---

## Placeholder Reference

| Placeholder | Replace with |
|---|---|
| `[Event or Series Label]` | Short uppercase label (e.g. "Spring 2026 Symposium") |
| `[Workshop Title]` | Main title |
| `[Subtitle or tagline]` | One-line framing sentence |
| `[Month Day, Year]` | Event date |
| `[Presenter Name]` | Speaker name(s) |
| `[Role or Affiliation]` | Title, institution, or both |
| `[Section Label]` | Short uppercase label for the content panel |
| `[Slide Title]` | h1 heading |
| `[Supporting subtitle...]` | h2 framing sentence |
| `[Column A/B Label]` | Tag above each column in a compare layout |
| `[Column A/B Heading]` | Bold heading inside each column |
| `[URL]` | Full URL for resource links |
| `[Link N Title]` | Display text for a resource link |
| `[Brief description]` | One-line description below a resource link |

---

## Step Grid Reference

```html
<div class="step-grid">
  <div class="step-row frag">
    <div class="step-num">1</div>
    <div class="step-text">[First point — use <strong>bold</strong> for key terms]</div>
  </div>
  <div class="step-row frag">
    <div class="step-num">2</div>
    <div class="step-text">[Second point]</div>
  </div>
</div>
```

Add or remove `.step-row.frag` blocks freely. The JS auto-fits font size if content overflows the stage height.

## Compare Layout Reference

```html
<div class="stageCompare">
  <div class="col">
    <span class="col-tag">[Column A Label]</span>
    <div class="col-title">[Column A Heading]</div>
    <div class="col-sub">[Column A description]</div>
  </div>
  <div class="col">
    <span class="col-tag">[Column B Label]</span>
    <div class="col-title">[Column B Heading]</div>
    <div class="col-sub">[Column B description]</div>
  </div>
</div>
```

## Resources Layout Reference

```html
<div class="res-wrap">
  <div class="res-group">
    <div class="res-group-title">[Group Label]</div>
    <ul class="res-list">
      <li>
        <a href="[URL]" target="_blank" rel="noopener">[Link Title]</a>
        <div class="res-desc">[Brief description]</div>
      </li>
    </ul>
  </div>
</div>
```

---

## Adding a Slide

1. Copy the nearest `<section class="slide">` block of the same type
2. Set a unique `data-slide="kebab-case-id"` attribute
3. Set `aria-label="Slide N: [Title]"` — use the correct slide number
4. Renumber `aria-label` on every slide that comes after this one
5. Update `max="N"` on `<input id="slide-scrubber">` in the footer
6. Fill in content, replacing all `[bracketed placeholders]`
7. Sync `SLIDES.md`
8. Commit

## Removing a Slide

1. Delete the full `<section class="slide">...</section>` block
2. Renumber all downstream `aria-label` values
3. Update `max="N"` on `#slide-scrubber`
4. Sync `SLIDES.md`
5. Commit

---

## Navigation: How It Works

The JS at the bottom of `index.html` handles everything:

- **Arrow keys / Space / PageDown** → advance (reveal next fragment, then next slide)
- **ArrowLeft / PageUp** → go back (hide last fragment, then previous slide)
- **Home / End** → first / last slide
- **O or Escape** → toggle overview mode (grid of all slides)
- **Next/prev buttons** in footer
- **Scrubber** (range input) → drag or click to jump to any slide
- **Touch swipe** and **trackpad horizontal scroll** supported

Do not modify the `<script>` block unless the task explicitly requires changing navigation behavior.

---

## The Scrubber

The `<input type="range" id="slide-scrubber">` in the sticky footer is the navigation scrubber. After changing the total slide count, update its `max` attribute:

```html
<input type="range" id="slide-scrubber" min="1" max="6" value="1" aria-label="Navigate to slide">
```

Change `max="6"` to match the actual number of slides.

---

## Design Tokens

All tokens are in the `<style>` block at the top of `index.html` under `:root`. Use tokens, not raw hex values:

| Token | Role |
|---|---|
| `--bg` | Page/slide background |
| `--fg` | Primary text |
| `--muted` | Secondary/subdued text |
| `--accent` | Links, interactive highlights |
| `--part` | Labels, step numbers, group titles |
| `--stroke` | Subtle borders |

---

## Accessibility Checklist

Before committing:
- [ ] Every `<section class="slide">` has `aria-label="Slide N: [Title]"` and `tabindex="-1"`
- [ ] All slide numbers are sequential with no gaps
- [ ] `max` on `#slide-scrubber` matches the total slide count
- [ ] Every `<img>` has a descriptive `alt` (or `alt=""` for decorative)
- [ ] `#announcer` and the skip link are still present and untouched
- [ ] `.stage` panels have `aria-hidden="true"` (stage content is decorative/supplementary)

---

## Commit Conventions

- Short, lowercase message (e.g. `add slides 3-5 on tinkering framework`)
- No `Co-Authored-By` lines
- No sign-off trailers
- Sync `SLIDES.md` before committing
