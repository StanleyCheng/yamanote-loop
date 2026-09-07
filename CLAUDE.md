# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a self-contained single-file web application displaying a walking/running route of Tokyo's Yamanote Line (山手線) with all 30 stations marked as stamp rally checkpoints. The supplied 7 September 2026 route covers 41.563 km (displayed as 41.6 km), following JY01 Tokyo through JY30 Yurakucho and returning to Tokyo. Distances follow the track and exclude extra stamp access detours.

**Key files:**
- `index.html` — Complete application (HTML + CSS + JS). The route GeoJSON data is embedded directly in this file (~70KB of coordinate data).
- `yamanote_loop_07Sep26.gpx` — Original supplied track geometry; preserve its track points
- `JY_Station.txt` — Authoritative JY codes, visit order, Japanese/English names, stamp locations and coordinates
- `route.gpx` / `yamanote_optimized_running.gpx` / `stamp_locations.gpx` — Synchronized GPX exports with 30 JY-labeled station waypoints
- `stamp_point.json` / `stamp_point.md` — Stamp descriptions in JY order; JSON keys are JY codes
- `.gitignore` — Only ignores `.env`

## Architecture

- **No build system** — The app is a single HTML file that loads Leaflet from CDN (`unpkg.com/leaflet@1.9.4`)
- **No testing** — No test framework or test files
- **No npm/node dependencies** — Pure HTML/CSS/JS

The inline JavaScript sets up:
1. A Leaflet map centered on Tokyo with OpenFreeMap Positron by default and a Detailed OpenStreetMap switch inside the Stamp Rally panel
2. The full route as a polyline (`routeGeoJSON` GeoJSON object)
3. Circle markers for all 30 stations with numbered labels
4. Popup content showing station name (JP + EN), stamp location, prev/next station distances, cumulative distance
5. A collapsible header panel with route stats
6. A bottom station list grid for quick navigation

## Development Commands

This is a static HTML file — open `index.html` directly in a browser:

```bash
open index.html
```

Or serve locally:
```bash
python3 -m http.server 8000
# Then open http://localhost:8000
```

## Making Changes

**Route data:** The `routeGeoJSON` object embeds all track points from `yamanote_loop_07Sep26.gpx` without changing their order or coordinates. Calculate the total by summing haversine segment lengths (Earth radius 6371 km). Keep the embedded geometry and all three GPX exports in sync.

**Station data:** The `stationsData` array contains JY codes and lat/lon/name/stamp info for all 30 stations, ordered JY01–JY30 from `JY_Station.txt`. Preserve the matching Chinese stamp translations. The order matters — `stationDists[i]` calculations rely on the array index to find prev/next stations. Project each stamp coordinate onto the nearest track segment to obtain cumulative distance and route offset; Tokyo is distance zero and the final leg returns from Yurakucho to Tokyo.

**Styling:** CSS is inline in the `<style>` block. Main color is `#1e96fc` (Garmin blue). Popup styles use `.station-popup` class. Map background is dark navy (`#0f1923`).

**Map tiles:** The Stamp Rally panel switches immediately between OpenFreeMap Positron (`https://tiles.openfreemap.org/styles/positron`) and standard OpenStreetMap tiles (`https://tile.openstreetmap.org/{z}/{x}/{y}.png`). Only the active base layer is attached to the map; route, station and GPS layers stay in place. Attribution follows the active provider. OSM tiles use native zoom levels up to 19 and scale up to zoom 24.
