# deck-templates

A set of minimal, no-build slide deck templates for workshops and presentations. Open an `index.html` in a browser and you're presenting. No npm, no bundler, no server required.

## Templates

| Directory | Engine | Style |
|---|---|---|
| `workshop/` | Custom deck engine | Light split-screen: left content panel, right stage; carousels, step reveal, copyable exercise templates |
| `minimal/` | Custom deck engine | Dark two-panel: numbered fragment step-grids, side-by-side compare, resource lists, self-contained single file |

## Quickstart

```bash
git clone https://github.com/zmuhls/deck-templates.git
open index.html                     # macOS
xdg-open index.html                 # Linux
# Windows: just double-click index.html
```

No dependencies to install. Edit `index.html`, save, reload.

## Choosing a Template

- **`workshop/`** — best for multi-section workshops with screenshots, carousels, and copyable exercise templates. Light background, full nav bar with scrubber, accessible by default.
- **`minimal/`** — best for compact presentations with sequential step reveals and side-by-side comparison panels. Dark theme, system font stack, all CSS/JS inline, mobile-friendly.

## Slide Layouts (`workshop/`)

| Class | Description |
|---|---|
| `layout-split` | Left content panel + right stage panel |
| `layout-content` | Single column, light background |
| `layout-full-dark` | Centered, dark background |
| `layout-divider` | Section break, large heading |
| `layout-grid` | Card grid layout |
| `title-slide` | Opening/closing slide |

## Editing Slides

Each template uses commented slide blocks as a guide:

```html
<!-- ==========================================
     SLIDE 3 — TWO-COLUMN SPLIT (layout: layout-split)
     Use for: Concept explanation with visual
     ========================================== -->
<div class="slide layout-split" ...>
  <div class="content">
    <span class="label label-cyan">The Basics</span>
    <h2>[Topic Title]</h2>
    <p>[Your paragraph here]</p>
  </div>
  <div class="stage">
    <!-- image, diagram, or carousel -->
  </div>
</div>
```

Replace `[bracketed placeholders]` with your content. Duplicate a slide block to add slides; delete to remove.

## Accessibility

All templates are built to WCAG 2.1 AA:
- Every slide needs `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`
- When adding or removing slides, renumber `aria-label` values and update the scrubber's `aria-valuemax`
- Decorative emoji use `aria-hidden="true"`
- `#slide-announcer` provides live-region narration for screen readers

## Deploying

These are static files. Any host works:

```bash
# GitHub Pages (push and enable Pages in repo settings)
git push origin main

# Local server (if you need relative paths to behave)
python3 -m http.server 8000
npx serve .
```

## Working with a Coding Agent

See `AGENTS.md` for instructions on delegating slide edits to Claude Code, Codex, or other CLI agents.

## Conventions

- No em dashes in slide content
- Commit messages: short, lowercase, no sign-off
- `SLIDES.md` in each template is a markdown mirror of `index.html` — keep in sync after edits
- Each template has a `CLAUDE.md` with per-template agent guidance
