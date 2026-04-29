# Repository Guidelines

## Project Structure & Module Organization

This repository is a self-contained static web map for the Yamanote Line stamp rally route.

- `index.html` contains the full application: HTML, inline CSS, inline JavaScript, embedded GeoJSON route data, station metadata, and Leaflet map setup.
- `route.gpx` and `yamanote_optimized_running.gpx` are GPX route exports. Keep them in sync when the route geometry changes.
- `stamp_point.json` and `stamp_point.md` store stamp checkpoint source data.
- `CLAUDE.md` contains additional implementation notes for AI coding agents.

There is no `src/`, package manager, build pipeline, or generated asset directory.

## Build, Test, and Development Commands

- `open index.html` opens the app directly in a browser on macOS.
- `python3 -m http.server 8000` serves the repo locally; visit `http://localhost:8000`.
- `git diff` reviews local changes before committing.

The app loads Leaflet from `https://unpkg.com/leaflet@1.9.4`, so network access is required for a full map render unless the dependency is cached.

## Coding Style & Naming Conventions

Use plain HTML, CSS, and JavaScript. Keep formatting consistent with the existing file: two-space indentation in CSS/JS blocks, semicolons in JavaScript, and descriptive camelCase names such as `routeGeoJSON`, `stationsData`, and `stationDists`.

Route and station arrays are order-sensitive. When editing `stationsData`, update related distances in `stationDists` and visible route totals. Preserve Japanese station names and multilingual stamp text exactly unless correcting source data.

## Testing Guidelines

No automated test framework is configured. Verify changes manually in a browser after every UI, route, or data edit.

Check that the map loads, the route fits within the viewport, all 30 station markers render, station-list clicks open the correct popup, and the header toggle works on desktop and mobile widths.

## Commit & Pull Request Guidelines

Recent commits use short, imperative summaries, sometimes with an emoji prefix, for example `Changed speed to pace notation in header` or `Optimized 48.8km walkable route`. Keep commits focused on one visible change or data update.

Pull requests should include a brief description, what was manually tested, and screenshots for visual map or layout changes. Mention any route source changes, distance recalculations, or stamp-location data updates.

## Agent-Specific Instructions

Keep edits narrowly scoped. Do not introduce npm tooling, bundlers, or frameworks unless explicitly requested. If consulting documentation for Leaflet or other libraries, use Context7 MCP first as required by the repository instructions.
