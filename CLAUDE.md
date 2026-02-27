# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-page personal portfolio website for Joshua Anghel. The entire site lives in a single `index.html` file — no build system, no frameworks, no dependencies beyond Google Fonts.

## Development

**Serve locally:**
```bash
python3 -m http.server 8080
```
Then open `http://localhost:8080`. The `.claude/launch.json` is pre-configured to run this.

There are no build steps, no linting tools, and no tests — just open the file in a browser.

## Architecture

Everything is in `index.html`, organized in this order:

1. **`<head>`** — Google Fonts (Inter, Outfit), CSS custom properties (design tokens), all styles
2. **`<body>`** — HTML structure (nav, sections, footer), then a single `<script>` block at the end

### CSS Design Tokens (`:root`)
- `--bg`, `--surface`, `--surface-hover` — background layers
- `--text`, `--text-2`, `--text-3` — text hierarchy
- `--accent` (`#6366f1`), `--accent-glow` (`#818cf8`) — indigo accent color
- `--border`, `--border-highlight` — border opacity levels
- `--mouse-x`, `--mouse-y` — updated per-element in JS for the glow-on-hover effect

### Page Sections
- `#hero` — full-viewport landing with animated background text ("0→1")
- `.stats` — four-column stat bar between hero and about
- `#about` — bio + facts grid
- `#projects` — three project cards (`.project.card-tilt`)
- `#skills` — infinite scrolling marquee (two identical `.skills-wrap` divs for seamless CSS loop)
- `#contact` — CTA section with mailto link
- `<footer>`

### JavaScript (inline, end of `<body>`)
Four interactive behaviors, all vanilla JS with no libraries:
1. **Spotlight** — smooth-following radial gradient that tracks the mouse via `lerp()` + `requestAnimationFrame`
2. **Reveal animations** — `IntersectionObserver` adds `.visible` class to `.reveal` elements as they scroll into view (fires once, then unobserves)
3. **Navbar scroll** — toggles `.scrolled` class on `#navbar` when `scrollY > 50`
4. **Waveform** — dynamically generates 40 `.wave-bar` divs inside `#waveform-visual` with randomized animation delays/durations and a bell-curve height distribution
5. **Card tilt** — `mousemove` on `.card-tilt` elements applies a `perspective(2000px) rotateX/Y` transform (max ±3°)

### Key CSS Patterns
- `.reveal` → `.reveal.visible` — opacity/translateY entrance animation
- `.card-tilt` and `.fact` elements use `--mouse-x`/`--mouse-y` CSS vars (set in JS) for a per-card radial glow that follows the cursor
- Skills marquee loops via `animation: marquee` on two duplicate `.skills-wrap` containers
- Responsive breakpoint at `768px` collapses the projects grid and adjusts nav/hero padding
