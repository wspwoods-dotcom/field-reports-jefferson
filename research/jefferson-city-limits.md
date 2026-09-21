# City of Jefferson, Iowa — Corporate Limits Polygon

Researched 2026-09-20 for Field Reports (city pilot with Eric Fisher, City of Jefferson Park Department). Research only — nothing published or deployed.

## Recommendation

Use the **U.S. Census Bureau TIGER/Line 2025 incorporated-places file**, filtered to Jefferson, IA. Downloaded, extracted, and converted to GeoJSON in this pass.

| Item | Detail |
|---|---|
| Source | U.S. Census Bureau, Geography Division — TIGER/Line Shapefiles, 2025 vintage, `PLACE` (incorporated + census-designated places) |
| Direct download URL | https://www2.census.gov/geo/tiger/TIGER2025/PLACE/tl_2025_19_place.zip |
| Format downloaded | ESRI Shapefile (.shp/.dbf/.shx, zipped); converted copy below is GeoJSON |
| Coordinate system / projection | NAD 83 (EPSG:4269), geographic lat/lon degrees. Functionally identical to WGS 84 at this scale — safe to drop straight into Leaflet/Mapbox GL without reprojection. |
| License / usage terms | U.S. federal government work — public domain, no license, no attribution required. |
| Vintage / currency | 2025 TIGER/Line. Place boundaries are maintained through the annual Boundary and Annexation Survey (BAS), where cities report annexations/detachments to the Census Bureau. This is the most current *uniform, machine-readable* city-boundary source available. |
| Record found | `NAME = Jefferson`, `NAMELSAD = Jefferson city`, `GEOID = 1939450` (state 19, place 39450), `LSAD = 25` (incorporated place), `FUNCSTAT = A` (active government), land area 15.58 km² (~6.0 sq mi) |
| File produced | [jefferson-ia-city-limits.geojson](sandbox://workspace/greene-county-maintenance-app/research/jefferson-ia-city-limits.geojson) — single Polygon, 1 ring, 161 vertices, ring closed, 4.4 KB |
| Sanity check | Bounding box lon −94.4164…−94.3579, lat 41.9922…42.0383; centroid 42.0177, −94.3794 — matches Jefferson, Greene County, IA. Boundary is a single clean polygon, no holes, no slivers. |
| Completeness | Complete corporate-limits polygon for the city. One caveat: TIGER boundaries are maintained for statistical purposes; the Census disclaims them as a determination of jurisdictional authority. For a maintenance-reporting app's map overlay (not legal work), this is fine. |

## Fallback option — OpenStreetMap

- **OSM relation 129187** — verified to be the *city* boundary, not the township: tags are `boundary=administrative`, `admin_level=8`, `border_type=city`, `name=Jefferson`, with `wikidata=Q1769522` (Jefferson, Iowa).
- Tags/geometry retrievable via https://www.openstreetmap.org/api/0.6/relation/129187 (full geometry via Overpass if needed).
- License: ODbL — requires attribution ("© OpenStreetMap contributors") and share-alike on the derived database, which is heavier than the public-domain TIGER data.
- Community-maintained: usually kept in sync with TIGER/BAS updates, but with no update guarantee. Use only if the TIGER file is unavailable.

## Other candidate sources (not verified in this pass)

- **Iowa Geodata (geodata.iowa.gov)** — Iowa's state GIS data hub; likely carries a city-boundaries feature layer from Iowa DOT / DOT open-data portal. No direct download URL was confirmed here; worth checking if a more Iowa-specific layer is wanted.
- **Iowa DOT Open Data Portal** — Iowa DOT publishes city maps (PDF) and some GIS layers; a "city limits" polygon layer may exist there.
- **Greene County Beacon (Schneidercorp)** — parcel-level county GIS; could be used to cross-check the corporate limits against city parcel/taxing-jurisdiction boundaries, but Beacon exports are county-wide parcels, not a ready-made city polygon.
- **Iowa DNR NRGIS "Incorporated Cities of Iowa"** — last documented vintage 2010; too stale to prefer over 2025 TIGER.

## Notes for app integration

- The GeoJSON is 4.4 KB — small enough to bundle directly in the app for offline use, no tiling or simplification needed.
- Coordinates are lon/lat degrees; style as a boundary outline (no fill, or very light fill) so park pins and reports stay legible on top.
- If the city annexes land in the future, re-pull the current-year TIGER/Line `PLACE` file and re-extract `PLACEFP = 39450` — one command, same pipeline.
