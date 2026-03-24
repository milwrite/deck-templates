# deck-templates

Two minimal, no-build slide deck templates for workshops and presentations. Open an `index.html` in a browser and you're presenting. No npm, no bundler, no server required.

## Templates

| Directory | Style | Best for |
|---|---|---|
| `workshop/` | Light split-screen: left content panel, right stage; carousels, step reveal, copyable exercise templates | Multi-section workshops with screenshots and hands-on activities |
| `minimal/` | Dark two-panel: numbered fragment step-grids, side-by-side compare, resource lists; single self-contained file | Compact talks, roundtables, sequential step reveals |

## Quickstart

```bash
git clone https://github.com/zmuhls/deck-templates.git
cd deck-templates/workshop      # or minimal/
open index.html                 # macOS
xdg-open index.html             # Linux
# Windows: double-click index.html
```

No dependencies to install. Edit `index.html`, save, reload.

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

Replace `[bracketed placeholders]` with your content. Duplicate a slide block to add slides; delete to remove. See `CLAUDE.md` in each template directory for the full agent-ready guide.

## Slide Layouts (`workshop/`)

| Class | Description |
|---|---|
| `title-slide` | Opening/closing slide |
| `layout-split` | Left content panel + right stage panel |
| `layout-content` | Single column, light background |
| `layout-full-dark` | Centered, dark background |
| `layout-divider` | Section break, large heading |
| `layout-grid` | Two-column card grid |

## Slide Types (`minimal/`)

| Stage pattern | Use for |
|---|---|
| `.step-grid` > `.step-row.frag` | Sequential numbered reveals |
| `.stageCompare` > two `.col` divs | Two-way contrasts |
| `.res-wrap` > `.res-group` | Links and references |
| `.stage-fill` > `iframe` | Live embed or demo |

## Accessibility

Both templates are built to WCAG 2.1 AA. Every slide needs the correct ARIA attributes — see `CLAUDE.md` for exact requirements. When adding or removing slides, renumber `aria-label` values and update the scrubber total.

## Working with a Coding Agent

See `AGENTS.md` for instructions on delegating slide edits to Claude Code, Codex, or other CLI agents.

## Deploying

Static files — any host works:

```bash
# GitHub Pages: push and enable in repo settings
git push origin main

# Local preview
python3 -m http.server 8000
npx serve .
```

## Conventions

- No em dashes in slide content
- Commit messages: short, lowercase, no sign-off
- `SLIDES.md` in each template is a markdown mirror of `index.html` — keep in sync after edits
