# Greg's Bingo Generator — Current Features

## Card Generator (Tab 1)

### Song Input
- Free-form textarea, one song per line
- 80+ default songs pre-loaded (mix of EDM, Latin, pop, rock)
- Live song counter badge with color states: grey (insufficient), yellow (warning), green (OK)
- Dynamic hint text showing required count and grid size

### Card Configuration
- **Number of cards**: 1–9999
- **Grid size**: Auto / 3×3 / 4×4 / 5×5 / 6×6 / 7×7
  - Auto mode selects grid based on song count
  - Required song counts: 3×3→8, 4×4→15, 5×5→24, 6×6→35, 7×7→48 (all assume free space)
- **Free Space**: Optional center cell with customizable label text
- **Card Title**: Fully editable; default "♫ Bingo Musical ♫"

### Card Generation Algorithm
1. Parse songs from textarea (trim, filter blank lines)
2. Determine grid size
3. Calculate needed count (cells − 1 if free space)
4. Validate: show error if insufficient songs
5. For each card: Fisher-Yates shuffle the full song list, take the first N songs
6. Build DOM card: header (title + card #), grid (cells), footer
7. Free space placed at exact center index (`Math.floor(cells/2)`)

### Generated Card Display
- Cards rendered in a responsive auto-fill grid (≥360px per column)
- Card number zero-padded to 3 digits (Card #001)
- `big-text` CSS class applied to cells with ≤6 character titles

### Card Design
- White card with dark blue header gradient
- Blue-tinted cell backgrounds with blue border
- Footer: "Private Use · Greg's Bingo Generator"
- Fonts: Playfair Display (title), DM Sans (cells)

### Printing (Bingo Cards)
- 2 columns, 2 rows (4 cards per print sheet) — but currently broken (see KNOWN_PROBLEMS.md)
- Letter paper, 0.4in margins
- Body class toggle method: adds `print-cards` before `window.print()`

## Live Caller Board (Tab 2)

### Song Tiles
- All songs displayed as clickable tiles
- Click to mark called (green strikethrough, reduced opacity)
- Click again to unmark
- Hover scale animation

### Now Playing Spotlight
- Large gradient text display showing last called song
- Fade-in animation on call
- "nowPlayingPulse" keyframe animation (4s loop) on active song

### Statistics Row
- Called count, Remaining count, Total count
- Live update on each toggle

### Search / Filter
- Real-time search filters visible tiles
- Matched tiles highlighted with accent border
- Search result count shown
- Reset clears search

### Reset
- Clears all called songs
- Resets spotlight to "—"
- Clears search field

### Win Pattern Panel
- Dropdown with 4 options: One Line, 4 Corners + Center, Perfect Cross, Full Card
- MP4 video preview auto-plays, loops, muted
- Video switches on dropdown change (pause → change source → load → play)
- **Not yet translated** (EN only labels, no i18n hook-up)

## Caller's Card (Tab 3)

### Display
- White document-style card
- Title synced with the Card Title input
- All songs displayed as tiles in a responsive grid
- Alternating even-row background

### Instructions
- Printed usage instructions: print two copies, cut one for drawing

### Printing
- Body class method: adds `print-caller` before `window.print()`
- Hides topbar, shows only the document

## Language Support (EN/ES)

- Fixed toggle button top-right (hidden on print)
- `setLang()` updates: page title, header, tabs, all labels, button text, placeholders, stats, caller board UI, caller's card UI
- Grid size select options update on language switch
- Song count hint updates on language switch
- **Win Pattern panel labels not translated** (known gap)

## Default Song List

80+ songs across genres:
- EDM/Electronic: Harlem Shake, Titanium, Clarity, Wake Me Up, etc.
- Latin: Danza Kuduro, Waka Waka, La Gozadera, La Camisa Negra, etc.
- Pop/Rock: Bad Romance, Wonderwall, Everlong, Sabotage, etc.
- Classic dance: The Ketchup Song, Dragostea din tei, etc.
