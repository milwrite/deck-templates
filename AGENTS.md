# AGENTS.md

This file tells Claude Code, Codex, and other CLI agents how to work with this repository. Each template directory has its own `CLAUDE.md` with complete per-template guidance — this file covers the repo-level workflow.

---

## Skills

Two skills live in `skills/` at the repo root. Read them before working on slides.

- **`skills/deck-editor/SKILL.md`** — edit protocol for `index.html`: line edits, slide add/remove with renumbering, scrubber total, SLIDES.md sync, validation checklist. This is the canonical reference for all slide edits regardless of which template you're working in.
- **Frontend design** — when touching CSS: use `:root` tokens only, no raw hex values, preserve contrast ratios, and keep `prefers-reduced-motion` rules intact.

---

## The Task

You are building a slide deck from a template. The flow is:

1. Clone or copy a template directory into a new repo
2. Read the template's `CLAUDE.md` fully before touching anything
3. Fill in `index.html` by replacing `[bracketed placeholders]` with real content
4. Sync `SLIDES.md` to mirror the final content
5. Commit

That's it. No package installs. No build step. Open `index.html` in a browser when done.

---

## Choosing a Template

| Directory | Style | Best for |
|---|---|---|
| `dynamic-light/` | Light split-screen, carousels, exercise templates | Multi-section workshops with screenshots and hands-on activities |
| `two-panel-dark/` | Dark two-panel, fragment step-grids, single file | Compact talks, roundtables, sequential reveals |

When in doubt: `dynamic-light/` for structured multi-part workshops; `two-panel-dark/` for everything else.

---

## Repository Structure

```
deck-templates/
├── dynamic-light/
│   ├── index.html          ← source of truth — all slide content lives here
│   ├── SLIDES.md           ← markdown mirror — keep in sync after edits
│   ├── CLAUDE.md           ← full agent guide for this template (read this first)
│   ├── css/
│   │   ├── styles.css      ← design tokens, layout, all components
│   │   ├── responsive.css  ← breakpoint overrides
│   │   └── animations.css  ← keyframes + prefers-reduced-motion
│   ├── js/
│   │   ├── tabs.js         ← tab component
│   │   ├── carousel.js     ← auto-advancing carousel
│   │   ├── scrubber.js     ← scrubber timeline + ARIA
│   │   └── deck-engine.js  ← core navigation (load last — do not reorder)
│   └── images/             ← slide images go here: slideN-letter.png
└── two-panel-dark/
    ├── index.html          ← the only file — all CSS and JS are inline
    ├── SLIDES.md           ← markdown mirror — keep in sync after edits
    └── CLAUDE.md           ← full agent guide for this template (read this first)
```

---

## Cloning and Setup

```bash
# Clone the repo
git clone https://github.com/zmuhls/deck-templates.git

# Copy your chosen template into a new project repo
cp -r deck-templates/dynamic-light/ ~/my-new-talk
cd ~/my-new-talk
git init && git add . && git commit -m "init from deck-templates/workshop"
```

No install step. Open `index.html` directly in a browser.

---

## Agent Workflow

### Claude Code (recommended)

```bash
cd dynamic-light/    # or two-panel-dark/
claude --permission-mode bypassPermissions --print 'Your task here'
```

Claude Code reads `CLAUDE.md` automatically. Do not paste the whole guide into your prompt — just state the task. Reference `CLAUDE.md` in your prompt if you need the agent to follow a specific convention.

**Example prompts:**

```
Fill in the deck for a 90-minute workshop on retrieval-augmented generation. Use the slide structure already in index.html. Replace all [bracketed placeholders] with appropriate content. Keep SLIDES.md in sync. Follow all conventions in CLAUDE.md.
```

```
Add a two-column comparison slide after slide 5 contrasting zero-shot and few-shot prompting. Use layout-split. Follow conventions in CLAUDE.md.
```

```
Replace the carousel on slide 4 with a screenshot-placeholder block. The image isn't ready yet. Update alt text and caption. Sync SLIDES.md.
```

### Codex

```bash
cd dynamic-light/    # or two-panel-dark/
codex exec --full-auto 'Fill in the deck for a workshop on AI in higher education. Replace all placeholders with real content. Follow CLAUDE.md conventions.'
```

---

## Universal Rules for All Agents

These apply to both templates. The per-template `CLAUDE.md` adds template-specific details.

1. **Read `CLAUDE.md` before editing anything.** Both templates have complete guidance including layout names, component patterns, accessibility requirements, and step-by-step task checklists.

2. **`index.html` is the only source of truth for slide content.** `SLIDES.md` is a mirror — keep it synced, but if there's a conflict, `index.html` wins.

3. **Accessibility is non-negotiable.** Every slide element needs the correct ARIA attributes. When you add or remove slides, renumber all downstream `aria-label` values and update the scrubber's total count. Details in each template's `CLAUDE.md`.

4. **No em dashes.** Use commas, colons, or restructure the sentence.

5. **Commit conventions:** short lowercase messages, no `Co-Authored-By`, no sign-off trailers.

6. **Do not restructure files.** Don't add new CSS files, rename JS modules, or change the load order of scripts. The templates are designed to be edited in place.

7. **Images** go in `images/` using `slideN-letter.png` naming (e.g. `slide4-a.png`). If an image isn't available, use a `<div class="screenshot-placeholder">` — both templates have this component.

8. **Use design tokens, not raw hex values.** Both templates define color tokens in `:root`. Prefer `var(--accent-cyan)` over `#4ECDC4`.

---

## Common Tasks

### Add a slide

1. Find the right insertion point in `index.html`
2. Copy the nearest slide block of the same type
3. Update `aria-label="Slide N: ..."` on this slide and all that follow
4. Update the scrubber total (see each template's `CLAUDE.md` for the exact attribute to change)
5. Fill in content
6. Sync `SLIDES.md`
7. Commit

### Remove a slide

1. Delete the full slide block
2. Renumber downstream `aria-label` values
3. Update the scrubber total
4. Sync `SLIDES.md`
5. Commit

### Add a step-reveal element (`dynamic-light/`)

Add `class="step-hidden" data-step` to any element. The deck engine reveals these one at a time on forward advance and resets them when leaving the slide.

### Add a fragment reveal (`two-panel-dark/`)

Add `class="frag"` to any element inside `.stage`. Fragments reveal one at a time on advance; navigating back reveals all of them at once.

### Add a carousel (`dynamic-light/` only)

```html
<div class="carousel" data-interval="8000">
  <div class="carousel-item active">
    <img src="images/slideN-a.png" alt="[Alt text]">
    <div class="carousel-caption"><strong>[Step]</strong><br>[Caption]</div>
  </div>
  <div class="carousel-item">
    <img src="images/slideN-b.png" alt="[Alt text]">
    <div class="carousel-caption"><strong>[Step]</strong><br>[Caption]</div>
  </div>
  <div class="carousel-dots"></div>
</div>
```

`data-interval` is auto-advance in milliseconds. Omit it to disable auto-advance.

### Change the color theme

- **`dynamic-light/`**: edit `:root` in `css/styles.css`
- **`two-panel-dark/`**: edit `:root` in the `<style>` block at the top of `index.html`

Key tokens: `--bg-light` / `--dark-bg`, `--accent-cyan`, `--accent-blue`, `--text-dark`, `--text-muted`.

---

## Deployment

```bash
# GitHub Pages: push and enable in repo settings (source: main branch, root dir)
git push origin main

# Local preview
python3 -m http.server 8000
# visit http://localhost:8000/dynamic-light/  or  http://localhost:8000/two-panel-dark/
```
