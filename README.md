# Field Reports — City of Jefferson (pilot)

A **separate app**, not a tab: the white-label twin of [Field Reports](https://wspwoods-dotcom.github.io/field-reports/) (Greene County), built for the City of Jefferson, Iowa. Pilot tester: **Eric Fisher** ("Fisher"), the city's Parks & Cemetery Superintendent (Park Maintenance Building, 104 N. Olive St., 515-386-4173).

- **Planned live URL:** https://wspwoods-dotcom.github.io/field-reports-jefferson/ (repo: `field-reports-jefferson`) — **not yet deployed.**
- **Own data:** localStorage key `field-reports-jefferson-db-v1`; own service-worker cache `field-reports-jefferson-shell-v1`. Because both apps live under one GitHub Pages origin, distinct keys are mandatory — they are set.
- **What's inside (same core as Greene v17):** Incident/FEMA tab with all six cost types + incident CSV, 45-second GPS first-fix acquire + resilient follow, shared category set with per-org toggles, crew wages, rate table.

## Still needed from Tanner / Eric before this is real

1. **City logo** — drop the real City of Jefferson logo at `assets/jefferson-city-logo.png`. The header and the service-worker precache already point at that exact path; the app must not deploy before the file exists (a missing precache entry makes the sw.js install fail).
2. **Facility-list review** — the 10 entries in `js/app.js` `DEFAULT_PARKS` are a DRAFT FOR TANNER/ERIC REVIEW (sourced from cityofjeffersoniowa.org; Head Park and St. Joseph's Cemetery coordinates are approximate). Nothing is authoritative until Eric confirms it.
3. **Rate table** — Jefferson city-specific rates still a TODO (currently shares the default table).
4. **App icons** — `assets/icon-192.png` / `icon-512.png` are still the Greene County shield. Regenerate from the Jefferson logo once it's supplied (ImageMagick: `magick jefferson-city-logo.png -resize 192x192 icon-192.png`, etc.).
5. **Emergency contact** — Eric Fisher's number goes into `emergency.phone` in `js/orgs.js` when Tanner approves; that's what reveals the guarded SOS header button.

## Files

- `index.html` — title/header branding for City of Jefferson
- `manifest.json` — name "Field Reports — City of Jefferson"
- `js/app.js` — Jefferson facilities draft, own DB key, Jefferson org default
- `js/orgs.js` — Jefferson-only org config (city-limits TIGER/Line 2025 boundary, emergency slot, incident module on)
- `sw.js` — cache `field-reports-jefferson-shell-v1`
- `DEVELOPER-HANDBOOK.md` — full continuity doc for the next developer
- `docs/fema-export-spec.md` — FEMA export spec (copied from Greene build)
