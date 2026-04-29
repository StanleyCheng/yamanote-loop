# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a self-contained single-file web application displaying a walking/running route of Tokyo's Yamanote Line (山手線) with all 30 stations marked as stamp rally checkpoints. The route covers 48.8km on real walkable roads.

**Key files:**
- `index.html` — Complete application (HTML + CSS + JS). The route GeoJSON data is embedded directly in this file (~70KB of coordinate data).
- `route.gpx` / `yamanote_optimized_running.gpx` — GPX exports of the route
- `.gitignore` — Only ignores `.env`

## Architecture

- **No build system** — The app is a single HTML file that loads Leaflet from CDN (`unpkg.com/leaflet@1.9.4`)
- **No testing** — No test framework or test files
- **No npm/node dependencies** — Pure HTML/CSS/JS

The inline JavaScript sets up:
1. A Leaflet map centered on Tokyo with dark tiles
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

**Route data:** The `routeGeoJSON` object (starts around line 82) contains all waypoints. This was generated via OSRM foot routing. To regenerate, use the OSRM demo server or a local OSRM instance with foot profile.

**Station data:** The `stationsData` array (around line 95) contains lat/lon/name/stamp info for all 30 stations. The order matters — `stationDists[i]` calculations rely on the array index to find prev/next stations.

**Styling:** CSS is inline in the `<style>` block. Main color is `#1e96fc` (Garmin blue). Popup styles use `.station-popup` class. Map background is dark navy (`#0f1923`).

**Map tiles:** Uses Carto's dark_matter tiles via `https://basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png`