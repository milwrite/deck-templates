# Skill: Slide Deck Editor

## Purpose

Handle single `index.html` slide deck files: conduct precise line edits that replace slide content, respond to copyedit requests, add/remove/reorder slides, and keep `SLIDES.md` in sync.

## When to Use

- User pastes or references a deck-templates `index.html` and asks for changes
- Slide content edits (titles, body text, labels, prompts)
- Layout changes (switching a slide from `layout-split` to `layout-content`)
- Adding or removing slides (with renumbering)
- Fixing accessibility attributes
- Generating or updating the `SLIDES.md` mirror

## Edit Protocol

### Minor Edits (copyedits, wording changes)

Use the `edit` tool with exact old/new text matching:

```
OLD: <h2>What Are System Prompts?</h2>
NEW: <h2>Crafting Effective System Prompts</h2>
```

For multi-line replacements, include enough surrounding context to ensure a unique match.

### Slide Content Replacement

When replacing an entire slide's content, match from the opening comment block through the closing `</div>`:

```
OLD: <!-- SLIDE 3 -->
<div class="slide layout-split" ...>
  [entire old content]
</div>

NEW: <!-- SLIDE 3 -->
<div class="slide layout-split" ...>
  [entire new content]
</div>
```

### Adding a Slide

1. Insert the new slide HTML at the correct position
2. Renumber all subsequent slides' `aria-label` attributes
3. Update the scrubber's `aria-valuemax`
4. Update the slide counter display
5. Update `SLIDES.md` with the new slide entry

### Removing a Slide

1. Remove the slide's HTML block (comment header through closing `</div>`)
2. Renumber all subsequent slides
3. Update scrubber `aria-valuemax`
4. Update slide counter
5. Remove from `SLIDES.md`

## Slide HTML Structure

Every slide follows this pattern:

```html
<!-- ==========================================
     SLIDE N — TITLE (layout: layout-class)
     Use for: [description]
     ========================================== -->
<div class="slide layout-class" role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1">
  [content]
</div>
```

## Layout Classes

| Class | Use Case |
|-------|----------|
| `title-slide` | Opening/closing |
| `layout-full-dark` | Dark bg, centered (roadmaps, takeaways) |
| `layout-split` | Two-column: `.content` (left) + `.stage` (right) |
| `layout-content` | Single-column, light bg |
| `layout-divider` | Section break, dark bg, large heading |
| `layout-grid` | Use `layout-content` with `.grid-2` inside for card grids |

## Interactive Elements

| Element | HTML Pattern |
|---------|-------------|
| Step reveal | `class="step-hidden" data-step` on the element |
| Stream list | `<ul class="stream-list">` |
| Carousel | `<div class="carousel" data-interval="8000">` with `.carousel-item` children |
| Prompt block | `.prompt-block.prompt-bad` / `.prompt-mid` / `.prompt-good` |
| Copy template | `<pre id="template-N">` + `<button onclick="copyTemplate('template-N')">` |
| Tip box | `<div class="tip-box">` |
| Label | `<span class="label label-cyan">` |

## Validation Checklist (after every edit)

- [ ] All slides numbered sequentially in `aria-label`
- [ ] `aria-valuemax` on scrubber matches total slide count
- [ ] Slide counter text matches total (`1 / N`)
- [ ] `SLIDES.md` reflects the change
- [ ] No em dashes in content
- [ ] Decorative emoji have `aria-hidden="true"`
- [ ] New images have descriptive `alt` text
- [ ] `<strong>` and `<em>` used (never `<b>` or `<i>`)
