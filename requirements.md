# Broad requirements (Ben)
An app to help people manage their garden.

## User requirements (Ben)

1. Define details of their garden
   1. User creates a grid map of their garden, with each cell representing 1 square meter
   2. User enters location of garden
   3. App creates visual representation of garden grid
   4. App uses garden location to customise management advice based on local climate, legal requirements (e.g. noxious plant classification), seasons and time zones
2. Create an inventory of the plants in their garden
   1. User enters a plant name
   2. User enters a grid location
   3. User enters other relevant text about the plant
   4. App helps clarify plant type with user, asking questions if necessary
   5. App finds appropriate management information
   6. App represents each plant on the grid at its location
3. See monthly management plan
   1. App creates calendar of activities for plants based on season, date etc. Activities include:
      1. Pruning
      2. Feeding
      3. Watering
      4. Protection
   2. App shows location of plants requiring each activity on the grid
   3. App can show or link user to more information about each activity if required
4. See plant-based information
   1. App can display or link to standardised information about each plant
   2. App can show whether an activity is recommended currently (e.g. whether pruning the plant right now is ideal, acceptable, or harmful)

## Technical decisions (Claude)

### Req 1.1 — App startup behaviour

- If a garden is already configured, it loads automatically on launch
- New garden onboarding only triggers if no garden exists
- A "New garden" or "Load garden" option is always accessible for users who want to switch

### Req 1.2 — Garden location input and derived data

#### Input
Street address (typed by user). Provides sufficient precision for microclimate and elevation, which can vary significantly within a single suburb (e.g. elevated foothill properties vs. valley floor).

#### Location data the app derives from the address (ref. req 1.4)
- Hemisphere and seasons
- Climate zone (rainfall, humidity, heat)
- Frost risk dates and intensity
- Elevation-adjusted care timing
- Noxious weed classifications by state/council (LGA boundary)

#### Technical approach — three chained API calls, all free and keyless

1. **Geocoding** (address → lat/lng): OpenStreetMap Nominatim
   `https://nominatim.openstreetmap.org/search?q={address}&format=json`

2. **Elevation** (lat/lng → metres): Open-Meteo Elevation API
   `https://api.open-meteo.com/v1/elevation?latitude={lat}&longitude={lng}`
   Based on Copernicus DEM GLO-90 (90m resolution, worldwide, free licence).

3. **Reverse geocoding** (lat/lng → council/LGA): OpenStreetMap Nominatim
   `https://nominatim.openstreetmap.org/reverse?lat={lat}&lon={lng}&format=json`

No API keys required — appropriate for a single-file HTML app with no secure backend.

#### Confirmation step
After lookup, app displays derived values (hemisphere, elevation, council/LGA, climate zone) with an edit option for each field, allowing users to correct any inaccuracies before proceeding.

### Req 1.2a — New garden onboarding flow

1. User enters a street address
2. App looks up and displays the following derived information for confirmation:
   - Hemisphere
   - Climate zone
   - Elevation (metres)
   - Frost risk
   - Council/LGA (for noxious weed classifications)
3. User confirms or edits each field before proceeding

### Req 1.1a — Garden map setup

After confirming location, user proceeds to define their garden map.

[Introductory text to be written — approx. 2 paragraphs explaining what the garden map is and what the user needs to do to define it.]

Steps:
1. User enters block dimensions in square metres (width × height)
2. App displays a grid with those dimensions, with grid references marked
3. User defines which direction is north
4. User optionally paints garden beds and buildings onto the grid using the cell painting tool

Note: Map setup is optional — users can skip it entirely and proceed directly to plant inventory. Within the map, painting garden beds and buildings is also optional — plants can be placed on any cell regardless of its type.

### Req 1.2b — Plant inventory setup

After confirming the garden map (or skipping it), user proceeds to build their plant inventory.

[Introductory text to be written — approx. 2 paragraphs explaining the plant inventory and what the user needs to do.]

Steps:
1. The garden grid is always visible during inventory entry so the user can locate grid references
2. User can add a plant in either of two ways:
   - Type a plant name and grid reference directly
   - Select a cell on the grid, then add a plant to that cell
3. Plants already entered are shown on the grid at their grid reference
4. User can browse the list of plants already entered at any time

### Req 2.3 — Plant identification and data sources

#### Identification level required
Genus-level identification is sufficient for most management advice (pruning, feeding, watering, frost protection). Cultivar-level detail is only needed in specific cases (e.g. flower colour affecting hydrangea feeding).

#### Recommended data sources

1. **Atlas of Living Australia (ALA)**
   - Australian native plants, noxious weed classifications by state/LGA
   - Free API, no key required
   - `https://api.ala.org.au`

2. **GBIF (Global Biodiversity Information Facility)**
   - Global species taxonomy and occurrence data
   - Free API, no key required
   - `https://api.gbif.org/v1`

3. **Trefle**
   - Garden plant database designed for apps
   - Good coverage of common cultivated plants
   - Free tier available
   - `https://trefle.io/api/v1`

4. **PlantNet API**
   - Photo-based plant identification
   - Same database as the PlantNet app
   - Free tier available
   - `https://my.plantnet.org`

#### Recommended approach
Use ALA as the primary source for Australian context and noxious weed status. Use GBIF or Trefle for general plant management information. Fall back to PlantNet for photo-based identification where the user cannot identify a plant by name.

#### Confirmation flow
After user enters an approximate plant name, app queries data source and presents a short list of matches. User selects the correct match or triggers a clarifying question flow if no match is found.

### Req 2.4 — Plant record fields

Each plant record should include:
- Plant name (user-entered, approximate)
- Confirmed species (resolved via data source)
- Grid reference
- Notes (free text, optional) — for any additional information the user wants to record about that plant

### Req 3+4 — Management Flow

The Management Flow is the main screen of the app, displayed when:
- The user loads the app and a garden is already configured, or
- The user completes garden setup

#### Layout (mobile first, iPhone)

- Garden grid displayed at the top, full width
- If the grid is taller than it is wide, rotate it so the longest edge runs horizontally
- Below the grid: a panel showing either the Garden Inventory or the Garden Calendar
- A tab or toggle switches between Inventory and Garden Calendar views
- Garden Inventory is shown by default

#### Controls

- Button to edit the current garden
- Button to create a new garden

#### Garden Inventory panel (default, ref. req 2)

- Default view lists plants grouped by species, as per the 'By species' view in the prototype
- Each species entry shows the number of instances and their grid references
- Selecting a species highlights all instances on the grid
- Selecting an individual plant instance opens plant information
- A toggle allows switching to a flat list view sorted by grid reference

#### Garden Calendar panel (ref. req 3)

- Monthly view of care tasks for the current garden
- Tasks are based on the confirmed plant list and the garden location
- Selecting a task highlights the relevant plants on the grid
- Tasks link to or display more information about each activity

#### Plant information (ref. req 4)

Plant information is accessible from three entry points:
- Selecting a cell on the grid that contains a plant
- Selecting a plant in the Inventory panel
- Selecting a plant within a Calendar task

Each entry point opens a plant information view showing:
- Standardised information about the plant
- Whether each care activity is currently ideal, acceptable, or harmful

#### Note on Calendar task links
Each calendar task must support two distinct actions:
- Show all plants requiring that task highlighted on the grid
- Open plant-specific information for an individual plant within that task
