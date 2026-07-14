# Greg's Bingo Generator — Implementation Plan

## Phase 1: Documentation & Baseline (COMPLETE)
- [x] Read and understand full codebase
- [x] Create docs/ directory and all documentation files
- [x] Create backups/ with timestamped copy
- [x] Git init and baseline commit

## Phase 2: Print Redesign (HIGH PRIORITY)

### Goal
Reliable, professional-looking printed cards in three sizes.

### Print size options
| Mode | Layout | Page | Cards/page |
|------|--------|------|-----------|
| Economy | 2×2 grid | Letter portrait | 4 |
| Large Print | 3×1 grid | Letter landscape | 3 |
| Jumbo | 1×2 grid | Letter portrait | 2 |

### Implementation steps
1. Add print size selector (3-button toggle or dropdown) to the output section UI
2. Remove the static `@page` and `#cardsContainer` rules from `@media print`
3. Create `injectPrintStyle(mode)` JS function that dynamically writes a `<style id="print-style">` block with the correct `@page` size and grid layout before calling `window.print()`
4. After `window.print()` resolves, remove the injected style
5. Update `printBingoCards()` to accept a mode parameter

### Card layout changes (all modes)
- **Header**: Single thin row — `Title` left, `#001` right — no music notes, no subtitle
- **Footer**: Single line centered — `Eventos • Trivia • Bingo Musical • greg-sun.com`
- **Grid**: `flex: 1` on `.bingo-grid` with `min-height: 0` so it fills remaining card height
- **Cells**: Remove fixed `min-height`; let the grid determine cell height naturally

### Font scaling
- Calculate title font size from character length in `buildBingoCard()`
- Inject as inline style on the title element
- Thresholds: ≤12 chars → 1rem, ≤20 → 0.85rem, ≤30 → 0.75rem, ≤40 → 0.65rem, >40 → 0.56rem
- Cells: font size varies by print mode (injected in print style), with `word-break: break-word; hyphens: auto`

## Phase 3: Caller Board Improvements

### Now Playing
- Remove any implicit `white-space: nowrap` behavior from Bebas Neue
- Set `max-width: 90%; word-break: break-word; white-space: normal` on `.last-called-song`
- Ensure animation still fires (it's class-based, independent of text wrapping)

### Win Pattern Panel — i18n
1. Add `winPatternLabel`, `winLine`, `winCorners`, `winCross`, `winFull` keys to both EN and ES in `I18N`
2. Add `data-i18n` attribute to the win pattern `<label>` and an ID to the `<select>`
3. Update `setLang()` to update the label and all four `<option>` elements

### Win Pattern Panel — layout
- Ensure responsive: stacks gracefully on narrow screens
- Bilingual labels confirmed working

## Phase 4: General UI Polish

### App header
- Replace multi-element header with a single compact bar
- Height target: ≤70px
- Layout: brand text left, lang toggle integrated right (remove fixed-position toggle)

### Reduce section padding
- Current `.section { padding: 1.75rem }` — reduce to `1.25rem`

## Phase 5: Spotify Import

### Dependencies
- Spotify Web API (Authorization Code with PKCE flow — no server needed)
- Developer App already exists (Greg has credentials)

### Flow
```
Paste Playlist URL → Import → Clean → Preview → Generate
```

### Cleaning rules (strip from titles)
- Radio Edit, Radio Mix
- Remastered (+ year variants: Remastered 2011, 2023 Remaster, etc.)
- feat. / featuring / ft. (and content in parens after)
- Live (Live at X, Live Version)
- Acoustic, Acoustic Version
- Original Mix
- Motion Picture, OST, Soundtrack
- Mono, Stereo
- Parenthetical years: (2021), [2019]
- Duplicate spaces → single space
- Trim

### UI Components
- URL paste input + Import button
- Loading state while fetching
- Cleaning preview (before/after columns)
- Edit individual titles
- Delete songs
- Reorder (drag handle)
- Duplicate detection badge
- Confirm → populate songs textarea → Generate

## Phase 6: Testing (after each phase)

For each milestone, generate:
- 10 cards — spot-check grid, header, footer, font sizes
- 30 cards — check pagination in print preview
- 100 cards — performance check (DOM build time)

Verify:
- Printing (Economy, Large Print, Jumbo)
- Caller board toggle, search, reset
- Language switch (EN↔ES) — all text updates
- Win pattern video (all 4 patterns, both languages)
- Caller's card print
- No JS console errors

## Phase 7: Deployment

1. `git init` (done in Phase 1)
2. `git remote add origin <GitHub repo URL>`
3. Push to GitHub
4. Connect Vercel to repo (or use `vercel --prod`)
5. Open deployed URL, run smoke test
6. Fix any production-specific issues, redeploy

## File Change Log (per phase)

| Phase | Files Changed |
|-------|--------------|
| 1 | docs/*.md, backups/*.html |
| 2–4 | index.html (CSS print block, JS print functions, card builder, i18n) |
| 5 | index.html (new Spotify section, cleaning functions, review UI) |
| 6 | No file changes (testing only) |
| 7 | .github/ or vercel.json (if needed) |
