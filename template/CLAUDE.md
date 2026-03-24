# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Important Rules

- **Always use the frontend-design skill when making changes that impact aesthetics.** This applies to any visual/styling modifications.
- **All changes must be accessible by default.** Every new slide, component, or interactive element must meet WCAG 2.1 AA. This is not optional and not a follow-up task.
- **Never use Co-Authored-By lines in commits.**

## Project Overview

Workshop [N] of a 3-part CUNY AI Lab faculty series (system prompts → knowledge collections → agentic tools). Single-page HTML slide deck, no build system, no npm dependencies. Open `index.html` directly in a browser or visit `https://cuny-ai-lab.github.io/[REPO-NAME]/`

- **Sibling repos:** https://github.com/cuny-ai-lab/system-prompting and https://github.com/cuny-ai-lab/knowledge-collections — share the same deck engine, CSS, and JS; content conventions should stay consistent across all

## Architecture

Custom deck engine, no framework.

**Entry point:** `index.html` — all slides are `.slide` divs inside `#deck`. Slide content is the source of truth; `SLIDES.md` is a markdown mirror kept in sync manually after edits.

**JS modules** (loaded at bottom of `index.html` in this order):
1. `js/tabs.js` — tab component
2. `js/carousel.js` — auto-advancing image carousel; exposes `window.startCarousel`, `window.pauseCarousel`
3. `js/scrubber.js` — scrubber timeline in the nav bar with ARIA slider keyboard support; exposes `window.updateScrubber(current, total)`
4. `js/deck-engine.js` — core navigation with focus management and live-region announcements; exposes `window.deckEngine`, `window.goTo`, `window.next`, `window.prev`

**CSS files:**
- `css/styles.css` — layout, components, typography, design tokens (`:root` variables), `.sr-only` utility, focus-visible styles
- `css/responsive.css` — breakpoint overrides (1024px, 768px, 480px)
- `css/animations.css` — `@keyframes` definitions, `prefers-reduced-motion` override

**Inline script** in `index.html`: `copyTemplate(id)` — clipboard copy for exercise templates.

**Images:** All image assets in `images/`. Screenshots: `slide{N}-{letter}.png` naming.

## Slide Layouts

Each slide uses one layout class:
- `layout-split` — two-column: `.content` (left, light) + `.stage` (right, panel)
- `layout-content` — single-column, light background
- `layout-full-dark` — centered, dark background
- `layout-divider` — section break, dark, centered large heading
- `layout-grid` — grid layout with `.grid-2` card grid

## Progressive Reveal

1. **Step reveal** — elements with `class="step-hidden" data-step` are revealed one at a time on forward advance. Reset when leaving the slide.
2. **Stream bullets** — `<ul class="stream-list">` items animate in with staggered delay (200ms + 250ms per item) when the slide becomes active.

## Accessibility

Every `.slide` div needs: `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`. When adding a new slide, renumber all subsequent slides.

Key infrastructure:
- `#slide-announcer` — `aria-live="polite"` div updated on every navigation
- `aria-current="step"` — set on the active slide
- Nav bar is `<nav aria-label="Slide navigation">`
- Scrubber has `role="slider"` with ARIA value attributes
- Decorative emoji use `aria-hidden="true"`
- `.sr-only` and `.skip-link` utilities in `css/styles.css`
- `prefers-reduced-motion` in `css/animations.css` disables all animations

## Design Tokens

Colors: `--bg-light`, `--bg-panel`, `--dark-bg`, `--dark-bg-lighter`, `--code-bg`, `--text-dark`, `--text-muted`, `--light-text`, `--light-text-50`, `--accent-navy`, `--accent-blue`, `--accent-cyan`, `--accent-light-blue`, `--accent-red`, `--accent-amber`, `--accent-green`

Typography: `Fraunces` (headings), `DM Sans` (body), `JetBrains Mono` (code/prompts) — all from Google Fonts CDN.

## Conventions

- No em dashes anywhere in slide content
- Prompt quality progression: `.prompt-bad` / `.prompt-mid` / `.prompt-good` with `.prompt-label` badges
- Hands-on exercises: template in stage panel first, "Your turn" tip-box after
- `SLIDES.md` must stay in sync with `index.html` after every content edit
- Commit messages: short, lowercase, no sign-off
- Frame AI as a tool the instructor builds, not something students use for homework
