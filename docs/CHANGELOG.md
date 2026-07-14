# Greg's Bingo Generator — Changelog

## [Unreleased] — 2026-07-14

### Added
- `docs/` directory with PROJECT_OVERVIEW.md, CURRENT_FEATURES.md, KNOWN_PROBLEMS.md, IMPLEMENTATION_PLAN.md, CHANGELOG.md
- `backups/` directory with pre-refactor snapshot
- `assets/videos/` and `assets/images/` directories for future asset organization
- Git repository initialized
- Print size selector UI (Economy 4/page, Large Print 3/page, Jumbo 2/page)
- `injectPrintStyle(mode)` — JS-driven `@page` injection for correct page size and layout per print mode
- Bilingual Win Pattern panel labels (EN/ES) — `winPatternLabel`, `winLine`, `winCorners`, `winCross`, `winFull` added to I18N
- Print size label text added to both EN and ES in I18N
- `titleFontSize(title, mode)` helper — scales card title font by character count
- `setPrintMode(mode, btn)` — tracks selected print mode and updates button state

### Changed
- **Card header**: replaced multi-element emoji header with a single slim flex row (title left, #NNN right). No music notes. No subtitle row.
- **Card footer**: replaced generic "Private Use · Greg's Bingo Generator" with `Eventos • Trivia • Bingo Musical • greg-sun.com`
- **Print CSS**: removed static `grid-template-rows: 5in 5in` hack; replaced with dynamic JS-injected styles per mode
- **`.bingo-card`**: now `display:flex; flex-direction:column` so grid fills remaining height
- **`.bingo-grid`**: `flex:1; min-height:0` so grid expands to fill card without overflow
- **`.last-called-song`**: added `white-space:normal; word-break:break-word; max-width:min(90vw,620px)` — long song titles now wrap instead of overflowing
- **App header**: reduced padding and font size to reclaim vertical space
- **`rebuildCallerCard()`** and `cardTitle` listener: fallback unified to `'Bingo'` (was inconsistently `'♫ Bingo Musical ♫'`)
- **`setLang()`**: removed dead code (`document.querySelector('label[for-key="cardTitle"]') && null`); added win pattern i18n; added print size button i18n
- **`buildBingoCard()`**: uses new slim header format and footer text; applies `titleFontSize()` to each card title element
- **`resetCallerBoard()`**: now also removes `now-playing-active` class from spotlight

### Fixed
- Bottom row clipping on print (root cause: `grid-template-rows` only defined first two rows)
- Win Pattern dropdown labels not translated on language switch
- Inconsistent card title fallback across three code paths

---

## [v1.0.0] — Pre-documentation baseline

Initial single-file application. Features:
- Bingo card generator with 3×3 through 7×7 grids
- Live caller board with song spotlight and search
- Caller's card for printing
- EN/ES language toggle
- Win pattern video preview panel
- Default song list (80+ songs, mixed EDM/Latin/Pop/Rock)
- Basic print support (2-column layout, letter paper)
