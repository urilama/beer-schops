# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Beer Schops** — a single-page static website for a fictional craft beer bar. The entire site lives in one file: `index.html`, with no build system, bundler, or package manager.

## Structure

- `index.html` — all HTML, CSS (embedded `<style>`), and JavaScript (embedded `<script>`) in one file
- `image references/beer1.png` — beer card image asset (referenced via `src="image%20references/beer1.png"`)

## Development

Open `index.html` directly in a browser — no server required. For live reload during editing, any static file server works:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## Architecture

The page has a strict visual layering system (z-index hierarchy):

| Layer | z-index | Purpose |
|---|---|---|
| Splash screen | 2000 | Full-screen intro, fades out on load |
| Nav | 500 | Fixed top bar, slides in after splash |
| Main content | 2 | Sections: `#hero`, `#about`, `#menu`, `#contact` |
| Bubble canvas | 1 | Animated rising bubbles |
| Background (orbs, grid, noise) | 0–1 | Decorative ambient layers |
| Custom cursor | 9998–9999 | `.cur-dot` + `.cur-ring` (replaces native cursor) |

**Animation pattern:** Elements use the `.rv` class (reveal) — they start `opacity:0; transform:translateY(38px)` and transition to visible when `.on` is added by an `IntersectionObserver` in the JS. Stagger delays are controlled by `.rv-d1` through `.rv-d6`.

**Fonts (Google Fonts CDN):**
- `Audiowide` — headings/logo
- `Chakra Petch` — subheadings, CTAs
- `DM Sans` — body text
- `Share Tech Mono` — labels, badges, monospace accents

**Color tokens** are defined as CSS custom properties on `:root` (e.g. `--green: #00ff87`, `--blue: #00c8f0`, `--bg: #060c1a`). Always use these variables rather than hardcoding hex values.

**Glass card pattern:** `.glass` class provides the frosted-glass look used on beer cards and stat tiles. Hover state shifts to a green-tinted background.

## Content Sections

- `#hero` — full-viewport landing
- `#about` — two-column layout: text left, 2×2 stats grid right
- `#menu` — auto-fill grid of `.beer` article cards (add new beers here)
- `#contact` — info cards left, contact form right
