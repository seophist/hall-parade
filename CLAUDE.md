# Hall Parade Garden Management App

A garden management tool for a property in Kurrajong Heights, NSW, Australia.

## Project Overview

This app helps the owner manage a large, multi-section garden through:
- An interactive grid map of the garden (1 cell = 1 sq metre)
- A plant inventory tied to grid positions
- A seasonal care calendar with monthly activity planning
- Plant-specific care timing guidance (pruning, feeding, watering, protection)

Location context matters: all seasonal advice, plant care timing, and activity recommendations must account for the Southern Hemisphere calendar (Kurrajong Heights, NSW — approx. 33.5°S, temperate/subtropical highland climate).

## Key Files

| File | Purpose |
|------|---------|
| `GardenGridCalendar.html` | Main app — interactive garden grid + care calendar (combined single-page tool) |
| `gardenInventory.csv` | Plant inventory with common name, confirmed species, section ref, grid ref, and notes |
| `garden_grid.json` | 2D array representing the garden grid; cell values: `"G"` (garden bed), `"W"` (wall/structure), `"DG"` (driveway/gravel/hard surface) |
| `requirements.md` | User requirements document |

> Note: `garden_care_calendar_v3.html` is referenced in planning but may not yet exist as a separate file — the calendar is currently integrated into `GardenGridCalendar.html`.

## Architecture

**Single-file HTML apps** — the main tool is a self-contained HTML file with embedded CSS and JavaScript. No build system, no dependencies beyond a Google Fonts CDN link.

**Grid coordinate system** — columns are letters (A–AB+), rows are numbers (1–47+). Grid refs in the inventory follow this convention (e.g. `D3`, `N1`, `AB12`).

**Garden sections** — the inventory uses section refs (i through vi and beyond) to group planting areas.

**Grid JSON** — `garden_grid.json` is a 2D array (rows × columns, 0-indexed). Cell types:
- `"G"` — garden bed (plantable area)
- `"W"` — wall, path, or structure
- `"DG"` — driveway, gravel, or hard surface

## Plant Inventory

The CSV (`gardenInventory.csv`) contains ~50+ plants including:
- Trees: White birch (*Betula sp.*), White cedar (*Melia azedarach*), Chinese crab apple (*Malus sp.*)
- Shrubs: Camellia, Mock orange (*Philadelphus coronarius*), Grevillea, Weigela, Indian hawthorn, Rosemary
- Groundcovers/perennials: Agapanthus, Azalea, Hydrangea, Star jasmine, Violet, Silver ponysfoot
- Ferns: Sword fern (*Nephrolepis sp.*)

## Design Conventions

- **Dark theme** — background `#1a1a18`, panel `#22221f`, text `#e8e4dc`, accent green `#3a5e2f` / `#8fbe6a`
- **Font** — DM Mono (monospace), loaded from Google Fonts
- **Mobile support** — body gets `mobile-view` class via JS; cell size drops from 15px to 7px
- **No frameworks** — plain HTML/CSS/JS only

## Development Notes

- The repo working tree is currently empty (all files live in git history). Check out `main` or reference commits to access source files.
- Latest commit with all source files: `909bb1a` (Create requirements.md)
- The GitHub Pages site serves files at `https://seophist.github.io/hall-parade/`
- When adding plant care advice, always use Southern Hemisphere seasons (summer = Dec–Feb, winter = Jun–Aug)
- NSW noxious weed regulations may be relevant for certain species
