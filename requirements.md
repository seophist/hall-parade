# Broad requirements (Ben)
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

## Technical decisions (Claude)

### Req 1.2 — Garden location input and derived data

**Input:** Street address (typed by user). Provides sufficient precision for microclimate and elevation, which can vary significantly within a single suburb (e.g. elevated foothill properties vs. valley floor).

**Location data the app derives from the address (ref. req 1.4):**
- Hemisphere and seasons
- Climate zone (rainfall, humidity, heat)
- Frost risk dates and intensity
- Elevation-adjusted care timing
- Noxious weed classifications by state/council (LGA boundary)

**Technical approach — three chained API calls, all free and keyless:**

1. **Geocoding** (address → lat/lng): OpenStreetMap Nominatim
   `https://nominatim.openstreetmap.org/search?q={address}&format=json`

2. **Elevation** (lat/lng → metres): Open-Meteo Elevation API
   `https://api.open-meteo.com/v1/elevation?latitude={lat}&longitude={lng}`
   Based on Copernicus DEM GLO-90 (90m resolution, worldwide, free licence).

3. **Reverse geocoding** (lat/lng → council/LGA): OpenStreetMap Nominatim
   `https://nominatim.openstreetmap.org/reverse?lat={lat}&lon={lng}&format=json`

No API keys required — appropriate for a single-file HTML app with no secure backend.

**Confirmation step:** After lookup, app displa
