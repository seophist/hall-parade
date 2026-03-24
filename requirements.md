# Broad requirements
An app to help people manage their garden.
## User requirements
1. Define details of their garden
   1. User creates a grid map of their garden, with each cell representing 1 square meter
   2. User enters location of garden
   3. App creates visual representation of garden grid
   4. App uses garden location to customise management advice based on local climate, legal requirements (e.g. noxious plant classification), seasons and time zones
2. Create an inventory of the plants in their garden
   1. User enters a plant name
   2. User enters a grid location
   3. App helps clarify plant type with user, asking questions if necessary
   4. App finds appropriate management information
   5. App represents each plant on the grid at its location
3. See monthly management plan
   1. App creates calendar of activities for plants based on season, date etc. Activities include:
      1. Pruning
      2. Feeding
      3. Watering
      4. Protection
   2. App shows location of plants requiring each activity on the grid
   3. App can show or link user to more information about each activity if required
4. See plant-based information
   1. App can display or link to stanardised information about each plant
   2. App can show whether an activity is recommended currently (e.g. whether pruning the plant right now is ideal, acceptable, or harmful)

## Technical decisions

### Req 1.2 — Garden location input and derived data

**Input method**
The user enters a street address as free text. The app geocodes this to coordinates.

**Derived location data**
From the coordinates, the app derives:
- Hemisphere and season mapping (Southern vs Northern Hemisphere calendar)
- Köppen climate zone (to select appropriate care templates)
- Frost risk category (based on elevation and latitude)
- Elevation-adjusted care timing (e.g. later last-frost date at higher elevation)
- Noxious weed classifications for the user's Local Government Area (LGA)

**Technical approach**
Three chained keyless (no API key required) APIs:
1. **OpenStreetMap Nominatim** — geocodes the street address to latitude/longitude, and also provides reverse geocoding to confirm the suburb, state, and LGA
2. **Open-Meteo Elevation API** — accepts latitude/longitude and returns elevation in metres above sea level
3. **Open-Meteo Climate API** — accepts latitude/longitude and returns historical climate normals used to determine climate zone and frost risk

**Confirmation step**
After geocoding and derivation, the app presents a summary of all derived values to the user (e.g. coordinates, LGA, hemisphere, climate zone, elevation, frost risk) and allows them to edit any value before it is saved. This guards against geocoding errors and lets users override derived classifications.