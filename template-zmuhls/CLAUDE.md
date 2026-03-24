# CLAUDE.md — template-zmuhls

This file provides guidance to Claude Code and other CLI agents when working with this template.

## Important Rules

- **All changes must be accessible by default.** Every slide section needs `role` and `aria-label`. This is not optional.
- **Never use Co-Authored-By lines in commits.**
- **No em dashes in slide content.** Use commas, colons, or rewrite.

## Project Overview

A self-contained single-file slide deck template. Dark two-panel theme based on the zmuhls/cuny-ai-intro visual style. All CSS and JS are inline in `index.html` — no external files, no build step. Open directly in a browser.

## Architecture

Single file: `index.html` — all slides, all CSS (in `<style>`), all JS (in `<script>`) live here.

`SLIDES.md` is a markdown mirror of the slide content. Keep it in sync after edits.

## Slide Structure

Each slide is a `<section class="slide">` inside `<main>`. The minimal structure:

```html
<section class="slide" data-slide="unique-id" aria-label="Slide N: [Title]" tabindex="-1">
  <div class="content">
    <!-- label, h1, h2, date, body text -->
  </div>
  <div class="stage" aria-hidden="true">
    <!-- step-grid, stageCompare, res-wrap, iframe, or empty -->
  </div>
</section>
```

**Exceptions:**
- Title slide: `data-slide="title"` — special CSS rules apply; stage can hold an iframe/canvas embed
- Section break: add class `slide-break` — no `.stage` needed

## Slide Types

| Pattern | Stage content | Use for |
|---|---|---|
| Step grid | `.step-grid` > `.step-row.frag` | Sequential reveals, process steps |
| Compare | `.stageCompare` > `.col` × 2 | Two-way contrasts, frameworks |
| Resources | `.res-wrap` > `.res-group` > `.res-list` | Links, references |
| Section break | (no stage) | Part transitions |
| Embed | `.stage-fill` > `iframe` or `canvas` | Live visual, demo |
| Closing | `.step-grid` with discussion prompts | Q&A, next steps |

## Fragment Reveal

Add `class="frag"` to any element inside `.stage`. On each Space/→ press:
- The next hidden `.frag` gets `class="visible"` (opacity 0 → 1, slight translateY)
- Once all fragments are revealed, the next advance moves to the next slide
- Going back one slide reveals all fragments of the previous slide

## Accessibility Requirements

Every `<section class="slide">` must have:
- `aria-label="Slide N: [Title]"` — renumber when adding/removing slides
- `tabindex="-1"` — receives focus on navigation

When adding a slide: update `aria-label` on all downstream slides, and update `scrubber max="N"` in the footer.

When removing a slide: same renumbering requirement.

**Do not remove** `#announcer` (live region) or the skip link — they serve screen readers.

## The Scrubber

The `<input type="range" id="slide-scrubber">` in the footer controls navigation. After changing the total number of slides, update `max="N"` to match.

## Adding a Slide

1. Copy the nearest slide block of the same type
2. Set a unique `data-slide` attribute (kebab-case, descriptive)
3. Update `aria-label="Slide N: [Title]"` for this and all downstream slides
4. Update `scrubber max="N"` in the footer
5. Add content, replacing `[bracketed placeholders]`
6. Sync `SLIDES.md`
7. Commit

## Removing a Slide

1. Delete the `<section>` block
2. Renumber downstream `aria-label` values
3. Update `scrubber max="N"`
4. Sync `SLIDES.md`
5. Commit

## Design Tokens (in `<style>` `:root`)

```css
--bg      /* page/slide background */
--fg      /* primary text */
--muted   /* secondary/subdued text */
--accent  /* links and interactive highlights */
--part    /* labels, step numbers, group titles */
--stroke  /* subtle borders */
```

Do not use raw hex values for these — reference the tokens.

## Commit Conventions

- Short, lowercase messages
- No Co-Authored-By or sign-off trailers
- `SLIDES.md` must be synced before committing
