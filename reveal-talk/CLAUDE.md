# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Important Rules

- **Never use Co-Authored-By lines in commits.**
- **All changes must be accessible by default.** Maintain keyboard navigation and screen reader support.

## Project Overview

CUNY AI Lab workshop slide deck built on Reveal.js 4.5.0 (CDN). Single `index.html` file with inline styles. No local dependencies, no build system.

- **Live:** `https://cuny-ai-lab.github.io/[REPO-NAME]/`
- **Engine:** Reveal.js loaded from jsDelivr CDN
- **Sibling decks (custom engine):** system-prompting, knowledge-collections
- **Sibling decks (Reveal.js):** critical-play, gen-dev-foundations, vibe-coding-prototypes

## Architecture

**Single file:** `index.html` contains all slides, styles, and Reveal.js initialization.

**Slide structure:** Each slide is a `<section>` inside `.reveal > .slides`. Horizontal sections advance left/right. Nested `<section>` elements create vertical slide groups (down arrow navigation).

**Section classes** (for background grouping):
- `section-intro` — title/icebreaker slides (dark)
- `section-content` — concept/explanation slides (alt dark)
- `section-demo` — demo/live-coding slides (dark)
- `section-activity` — hands-on exercises (dark)
- `section-break` — section dividers (alt dark)
- `section-closing` — summary/resources/thank you (alt dark)

**Timing badges:** `<div class="timing">10 min</div>` on each slide for facilitator reference. Hidden on section-intro and section-break slides.

## Key Components

- **Two-column grid:** `.two-column` class on a div
- **Tables:** Standard HTML tables with themed th/td styles
- **Worksheet boxes:** `.worksheet-section` class
- **Checkbox lists:** `.checkbox-list` class on ul (adds checkbox prefix to li)
- **Citations:** `.citations` class for small reference text

## Navigation

- **Horizontal:** Left/right arrows or spacebar
- **Vertical:** Up/down arrows (within nested section groups)
- **Hash routing:** `index.html#/3` jumps to slide 3
- **Overview:** Esc key

## Conventions

- No em dashes in slide content
- `SLIDES.md` is a markdown mirror of slide content (manual sync after edits)
- Commit messages: short, lowercase, no sign-off
- Section classes must be set on every `<section>` for consistent backgrounds
- Timing values should sum to total workshop duration (typically 90 min)
- All CSS is inline in `<style>` block; no external stylesheets beyond Reveal.js CDN
