# First design briefing
- This section added by Ben. Do not change content in this section. Instead, re-state this section in your own words in a section below.

## General info
- Always ask clarifying questions before starting a new feature.

## Reference info
- gardenapp_v0.html
  - An early HTML prototype of the app
  - Use it as a high-level reference for the intended app design, or to look for clarification about particular requirements
  - Clarify any inferences or choices regarding this prototype with me before implementing
- gardenapp_v0_grid.json
  - Stores grid data for the example garden in the prototype
- gardenapp_v0_inventory.csv
  - Stores plant inventory data for the example garden in the prototype

## Architecture
- Web app must be built using Angular Material (material.angular.dev)

## Tech stack
- Framework: Angular 19 (standalone components)
- Component library: Angular Material (material.angular.dev)
- Language: TypeScript
- No server-side rendering required
- Single-page application
- Mobile first

## Location and seasonal context
All location-dependent behaviour — including hemisphere, seasons, climate zone, frost risk, elevation adjustments, and noxious weed classifications — must be derived dynamically from the street address entered by the user during garden setup (ref. Req 1.2). Nothing in this regard should be hardcoded.

The reference files (gardenapp_v0.html, gardenapp_v0_grid.json, gardenapp_v0_inventory.csv) contain data for an example garden and should be treated as illustrative only.

## Requirements
Full app requirements are in requirements.md. Read this before starting any work.

## Repo access
All project files are accessible via the following URL pattern:
`https://raw.githubusercontent.com/seophist/hall-parade/refs/heads/main/{filename}`

Key files:
- `https://raw.githubusercontent.com/seophist/hall-parade/refs/heads/main/requirements.md`
- `https://raw.githubusercontent.com/seophist/hall-parade/refs/heads/main/gardenapp_v0.html`
- `https://raw.githubusercontent.com/seophist/hall-parade/refs/heads/main/gardenapp_v0_grid.json`
- `https://raw.githubusercontent.com/seophist/hall-parade/refs/heads/main/gardenapp_v0_inventory.csv`