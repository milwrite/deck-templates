# AGENTS.md — Working with CLI Coding Agents

This file tells Claude Code, Codex, and other CLI agents how to work with this repository. If you're a human, it also tells you how to prompt agents effectively for slide work.

## Repository Structure

```
deck-templates/
├── split-light/            # Custom deck engine — light split-screen
│   ├── index.html          # All slides live here — source of truth
│   ├── SLIDES.md           # Markdown mirror of index.html — keep in sync
│   ├── CLAUDE.md           # Per-template agent guidance
│   ├── css/
│   │   ├── styles.css      # Design tokens, layout, components, typography
│   │   ├── responsive.css  # Breakpoint overrides
│   │   └── animations.css  # Keyframes + prefers-reduced-motion
│   ├── js/
│   │   ├── tabs.js
│   │   ├── carousel.js
│   │   ├── scrubber.js
│   │   └── deck-engine.js  # Core navigation — load last
│   └── images/
├── reveal-talk/            # Reveal.js — dark, timing badges, print-to-PDF
│   ├── index.html
│   ├── SLIDES.md
│   └── CLAUDE.md
└── dark-panel/             # Custom engine — dark two-panel, all inline
    ├── index.html          # All CSS and JS inline — single file
    ├── SLIDES.md
    └── CLAUDE.md
```

## Cloning and Setup

```bash
git clone https://github.com/zmuhls/deck-templates.git
cd deck-templates
# No install step. Open index.html in a browser.
```

For a new presentation, copy the relevant template directory into its own repo:

```bash
cp -r split-light/ ~/my-workshop    # or reveal-talk/ or dark-panel/
cd ~/my-workshop
git init && git add . && git commit -m "init from deck-templates"
```

## How to Prompt an Agent

### Claude Code (recommended)

```bash
cd split-light/    # or reveal-talk/ or dark-panel/
claude --permission-mode bypassPermissions --print 'Your task here'
```

Claude Code reads `CLAUDE.md` automatically. You do not need to paste the style guide into your prompt — just describe what you want.

**Example prompts:**

```
Add a new section divider slide after slide 6 for "Part II: Hands-On Work". Follow all conventions in CLAUDE.md.
```

```
Replace slides 3 through 5 with a single two-column layout comparing zero-shot and few-shot prompting. Add relevant placeholder content. Keep SLIDES.md in sync.
```

```
The carousel on slide 4 has three images. Add a fourth image placeholder (images/slide4-d.png) with caption "Step 4: Review the output".
```

### Codex

```bash
cd split-light/    # or reveal-talk/ or dark-panel/
codex exec --full-auto 'Add a closing slide with a thank-you heading and placeholder contact info. Follow conventions in CLAUDE.md.'
```

### Key Rules for All Agents

1. **Read `CLAUDE.md` first.** It has layout names, accessibility requirements, commit conventions, and component patterns. Do not guess — look it up.

2. **`index.html` is the source of truth.** All slide content lives there. `SLIDES.md` is a markdown mirror — update it after any content edit.

3. **Accessibility is not optional.** Every slide div needs `role="group" aria-roledescription="slide" aria-label="Slide N: Title" tabindex="-1"`. When you add or remove a slide, renumber everything downstream.

4. **Commit conventions:** short, lowercase messages. No `Co-Authored-By` lines. No sign-off trailers.

5. **No em dashes** in slide content. Use commas, colons, or rewrite the sentence.

6. **Don't touch `deck-engine.js` unless the task is explicitly about navigation behavior.** It manages focus, live regions, and step reveal — easy to break.

7. **When adding images**, use `images/slideN-letter.png` naming (e.g., `slide7-a.png`). Add accessible `alt` text. If the image doesn't exist yet, use a `<div class="screenshot-placeholder">` instead.

8. **Design tokens are in `css/styles.css` `:root`.** Prefer token references (`var(--accent-cyan)`) over raw hex values.

## Common Tasks

### Add a new slide

1. Find the right insertion point in `index.html`
2. Copy the nearest slide block of the same layout type
3. Replace content and increment `aria-label="Slide N: ..."` for this slide and all following
4. Update `aria-valuemax` on the scrubber container to the new total
5. Sync `SLIDES.md`
6. Commit

### Remove a slide

1. Delete the slide div block
2. Renumber all downstream `aria-label` values
3. Update `aria-valuemax` on the scrubber
4. Sync `SLIDES.md`
5. Commit

### Change the color theme

Edit `:root` in `css/styles.css`. The main tokens:

```css
--bg-light         /* slide background (light layouts) */
--dark-bg          /* dark layouts, title/closing slides */
--accent-cyan      /* primary accent — bullet dots, labels, progress bar */
--accent-blue      /* secondary accent — cards, roadmap items */
--text-dark        /* body text on light backgrounds */
--text-muted       /* secondary text */
```

### Add a step-reveal element

Add `class="step-hidden" data-step` to any element inside a slide. The deck engine will reveal these one at a time on forward navigation, and reset them when leaving the slide. Works on any element: `<h3>`, `<div>`, `.prompt-block`, etc.

### Add a carousel

```html
<div class="carousel" data-interval="8000">
  <div class="carousel-item active">
    <img src="images/slideN-a.png" alt="[Description]">
    <div class="carousel-caption"><strong>[Step Title]</strong><br>[Caption text]</div>
  </div>
  <div class="carousel-item">
    <img src="images/slideN-b.png" alt="[Description]">
    <div class="carousel-caption"><strong>[Step Title]</strong><br>[Caption text]</div>
  </div>
  <div class="carousel-dots"></div>
</div>
```

Place inside a `.stage` panel. `data-interval` is auto-advance in milliseconds; omit to disable auto-advance.

## Per-Template Notes

### `split-light/` (custom engine, light split-screen)
- JS load order matters: tabs → carousel → scrubber → deck-engine
- Inline `copyTemplate(id)` script in `index.html` handles clipboard for exercise templates
- Logo watermark: replace `src` in `<img class="logo-watermark">` or remove the element
- Best for: multi-section workshops, screenshot walkthroughs, hands-on exercises

### `reveal-talk/` (Reveal.js, dark)
- Add `data-timing="N"` (seconds) to each `<section>` to show facilitator timing badges
- Use nested `<section>` inside `<section>` for vertical slide groups
- Speaker notes: add `<aside class="notes">...</aside>` inside any section
- Print to PDF: append `?print-pdf` to the URL, then print from browser
- Best for: conference talks, faculty presentations, anything needing print output

### `dark-panel/` (custom engine, dark two-panel, all inline)
- All CSS and JS are inline in `index.html` — no separate files to manage
- Fragment reveal uses `.frag` class with a `visible` class toggled by JS
- Stage panels use absolute positioning at tablet+ breakpoints; leave `.stage` wrappers intact
- Swipe navigation and trackpad gestures built in
- Best for: compact presentations, roundtables, talks with progressive step reveals

## Deployment

```bash
# Push to GitHub Pages
git push origin main
# Then enable Pages in repo settings → source: main branch, root directory

# Preview locally
python3 -m http.server 8000
# Visit http://localhost:8000/split-light/  (or /reveal-talk/ or /dark-panel/)
```
