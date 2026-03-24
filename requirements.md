## Technical decisions

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
