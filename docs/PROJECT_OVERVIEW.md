# Greg's Bingo Generator — Project Overview

## Summary

A single-file, zero-dependency HTML application that generates printable musical bingo cards, runs a live caller board, and provides a printable caller's reference card. Written entirely in vanilla HTML/CSS/JS with no build step.

## Entry Point

`index.html` — the entire application. All CSS and JS are inline. No external dependencies except Google Fonts.

## Technology Stack

- HTML5 / CSS3 / Vanilla JavaScript (ES6+)
- Google Fonts: Bebas Neue, DM Sans, Playfair Display
- No framework, no build toolchain, no server required
- Runs directly from the filesystem or any static host (Vercel, GitHub Pages)

## Architecture

Single HTML file with three logical sections:

1. **Inline `<style>`** — all CSS tokens, layout, component styles, print media queries
2. **HTML body** — three tabs rendered as hidden/shown panels
3. **Inline `<script>`** — I18N strings, state, and all application logic

## Tab Structure

| Tab | Panel ID | Purpose |
|-----|----------|---------|
| Card Generator | `generatorPanel` | Input songs, configure options, generate and display bingo cards |
| Live Caller Board | `callerBoardPanel` | Call songs during the game, spotlight "Now Playing", track stats, win pattern video preview |
| Caller's Card | `callerCardPanel` | Printable master list for the game host |

## State

```js
let calledSongs = new Set();   // songs marked as called on the board
let currentSongs = [];         // full list of songs from textarea
let currentLang = 'en';        // active language (en | es)
```

## Languages

Full bilingual support: English (EN) and Spanish (ES). All UI strings live in the `I18N` object. The `setLang()` function updates all DOM text on switch. The Win Pattern panel labels are currently not translated (known issue).

## Video Assets

Four MP4 files ship alongside `index.html`, used for the Win Pattern preview:

| File | Pattern |
|------|---------|
| `win_one_line_v2.mp4` | One Line |
| `cardstyle_four_corners_center.mp4` | 4 Corners + Center |
| `cardstyle_perfect_cross.mp4` | Perfect Cross |
| `cardstyle_full_card.mp4` | Full Card |

## Print System

Print is triggered by adding a body class (`print-cards` or `print-caller`) before calling `window.print()`. The `@media print` block hides UI chrome and restructures the card container into a 2-column grid.

## Deployment

Static file. Works from `file://` or any static host. GitHub + Vercel deployment is planned.

## Directory Layout (post-refactor)

```
gregs_bingo_generator-main/
├── index.html              ← main application
├── index - V1.html         ← earlier version (reference only)
├── win_one_line_v2.mp4
├── cardstyle_four_corners_center.mp4
├── cardstyle_perfect_cross.mp4
├── cardstyle_full_card.mp4
├── assets/
│   ├── videos/             ← future: moved MP4 assets
│   └── images/             ← future: logo, branding images
├── backups/                ← timestamped HTML snapshots before major changes
└── docs/                   ← this documentation
```
