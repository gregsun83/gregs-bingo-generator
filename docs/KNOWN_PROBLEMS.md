# Greg's Bingo Generator — Known Problems

Last updated: 2026-07-14

---

## CRITICAL — Print Layout

### P1: Bottom row clipping on print
- **Location**: `@media print` block, `#cardsContainer` styles
- **Problem**: The current layout sets `grid-template-rows: 5in 5in` which defines only the first two rows. Cards beyond row 2 get auto-sized rows that may overflow the page or be clipped. The `.bingo-grid { flex: 1; }` rule also doesn't work correctly when the card container has no explicit height.
- **Root cause**: Mixed grid and flexbox approach with fixed `5in` row heights creates unpredictable behavior when more than 4 cards are printed.
- **Impact**: Any print run of more than 4 cards will likely clip cards or have misaligned rows.

### P2: No print size options
- **Problem**: Only one print layout (2-column, ~4 cards/page) with no way to choose Economy/Large Print/Jumbo.
- **Impact**: Players needing large print cannot use the tool effectively.

### P3: Oversized header on printed cards
- **Problem**: `bingo-card-header` has `padding: 0.85rem 1rem` and the title is `1.4rem` with music note decorations. Wastes ~0.8–1in of card height.
- **Impact**: Less room for the song grid; cells are cramped.

### P4: Oversized footer on printed cards
- **Problem**: Footer text "Private Use · Greg's Bingo Generator" is generic and takes unnecessary space.

### P5: Font too small in cells
- **Problem**: `font-size: 0.65rem` (about 7.8pt in print) is too small for casual players to read quickly under event conditions.

### P6: Cell padding insufficient
- **Problem**: `padding: 4px 3px` with `min-height: 50px` doesn't scale well to different grid sizes or print sizes.

### P7: No intelligent font scaling for title
- **Problem**: Long card titles (e.g. "90s Latin Hits Mega Mix Bingo 2026") overflow the header or are truncated with no graceful degradation.

---

## HIGH — Caller Board

### C1: "Now Playing" text truncated on long titles
- **Problem**: `.last-called-song` has `padding: 0 1rem` but no `max-width` and `white-space` is inherited (nowrap from Bebas Neue behavior). Long song titles overflow or push layout.
- **Location**: `.last-called-song` CSS rule (~line 218)

### C2: Win Pattern panel not translated
- **Problem**: The `<label for="winTypeSelect">Win Pattern</label>` and `<select>` options are hardcoded in English. `setLang()` does not update them.
- **Location**: HTML ~line 562–566, `setLang()` function ~line 1058

---

## MEDIUM — Code Quality

### D1: Dead code in setLang()
- **Location**: Line 1083
- `document.querySelector('label[for-key="cardTitle"]') && null;`
- This statement does nothing. The selector finds nothing (there is no `for-key` attribute in the DOM) and the `&& null` discards the result.

### D2: Caller card title fallback hardcodes "♫ Bingo Musical ♫"
- **Location**: Line 829 (`cardTitle` input listener) and `rebuildCallerCard()` line 1034
- The `rebuildCallerCard()` function falls back to `'♫ Bingo Musical ♫'` regardless of the actual card title field value. This should use the user's input or a generic default.

### D3: Inconsistent fallback between functions
- `generateCards()` falls back to `'Bingo'` (line 895)
- `rebuildCallerCard()` falls back to `'♫ Bingo Musical ♫'` (line 1034)
- `cardTitle` input listener updates `callerCardTitle` with same hardcoded fallback (line 829)
- Should all use the same sensible default.

### D4: `min-height: 50px` on cells is screen-only, not scaled to print
- CSS sets `min-height: 50px` for `.bingo-cell` globally. In print, this is fine for some layouts but too tall for economy (4/page) in landscape.

### D5: No CSS variable abstraction for card colors
- Card header blue gradient, cell background blue, and cell border blue are all hardcoded hex values (`#1a3a7a`, `#e8f0fb`, `#b8cce8`, etc.) rather than CSS variables. Makes theming or future customization brittle.

### D6: `hasFreeSpace` checkbox change listener checks `needed` incorrectly
- `updateSongCount()` computes `needed = grid*grid - (hasFreeSpace.checked ? 1 : 0)` but then passes `needed` to `needSongsHint(needed, grid)` — the hint function signature expects `(n, g)` where `n` is the count of songs, not needed. The hint text is therefore slightly off when free space is unchecked.

---

## LOW — UX

### U1: No PDF generation UI
- The print button says "Print / Save PDF" but there is no actual PDF export (browser print-to-PDF is the only path). No dedicated PDF library.

### U2: Spotify import not implemented
- Planned feature, not yet started.

### U3: Playlist review not implemented
- No ability to edit, delete, reorder, or deduplicate songs before generating.

### U4: `cardTitle` input listener doesn't update on paste or programmatic change
- Only `'input'` event; `'change'` or `'paste'` not covered (though `input` covers paste in modern browsers — low risk).

### U5: App header wastes vertical space on smaller screens
- The `<header>` block uses `padding: 2.5rem 1rem 0` with a large emoji, h1, and subtitle. On mobile this pushes content below the fold.

### U6: No keyboard navigation for caller board tiles
- Tiles are `div` elements with click handlers. Not focusable by keyboard, not accessible.

---

## FUTURE / OUT OF SCOPE NOW

- Spotify Web API integration
- Playlist review UI (edit/delete/reorder/deduplicate)
- PDF generation via library (jsPDF, html2canvas)
- Persistent state (localStorage) for caller board sessions
- Mobile-optimized caller view
- Custom color themes
- Card preview before bulk generation
