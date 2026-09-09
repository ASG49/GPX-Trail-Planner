# GPX Trail Planner

A single-file, no-build-step web app for plotting a hiking/trail route from
scratch by clicking points on a map, then exporting it as a standard GPX
file with altitude for every point. Runs entirely client-side in Chrome (or
any modern browser) — open the app file locally or via GitHub Pages.

**Live app:** https://asg49.github.io/GPX-Trail-Planner/

**Current version: v1.23**

## Features

- **Basemap choice** — OpenTopoMap, OpenStreetMap, Google Satellite, or
  Google Terrain, switchable from a dropdown top-right of the map.
- **Click to build a route** — each click drops the next sequential,
  numbered point; click on the trail line itself to insert a new point
  between two existing ones.
- **Drag to fine-tune** — drag any point marker to reposition it.
- **Automatic altitude** — every new point's elevation is looked up in the
  background (Open-Elevation, falling back to Open-Topo-Data's SRTM90m)
  and filled in as soon as it resolves. A manual "Fetch Elevations" button
  is also available for re-fetching after a drag.
- **Load GPX File** — re-import a track previously saved by this app.
  Points are rebuilt using the same marker logic as click-added points, so
  the full editing toolset (drag, insert-on-line, chart hover linkage,
  etc.) works identically on an imported track. Points missing `<ele>`
  get it auto-fetched. The map auto-fits to the imported track's bounds.
  (Built for tracks made in this app — dense, multi-hundred-point GPS
  recordings are better handled by a dedicated recorder/simplification
  tool, not this click-to-build editor.)
- **Location search** — jump to any place or address via OpenStreetMap's
  Nominatim geocoder.
- **Center on my location** — a round button under the zoom control
  centers the map on the browser's current geolocation; the map also
  tries to do this automatically on load.
- **Live stats** — running point count, total distance, and altitude-fetch
  progress.
- **Altitude vs. distance profile** — a semi-transparent chart overlay,
  bottom-right of the map, rendered as inline SVG (no chart library):
  - Cubic-spline (Catmull-Rom) curves for both series, so the lines pass
    smoothly through every data point.
  - A secondary Y2 axis for slope %, computed by looking ahead until each
    segment covers at least a minimum real distance — this avoids
    spurious spikes from elevation-grid noise on closely-spaced points.
    That minimum is adjustable (25 / 50 / 75 / 100m, default 75m) via a
    dropdown in the sidebar, since shorter tracks benefit from a smaller
    minimum and longer/noisier ones from a larger one.
  - Solid dots at each real altitude point on the elevation curve.
  - Two-way hover linkage: hovering a map marker draws a red line on the
    chart at that point's distance; hovering a chart altitude dot shows a
    red arrow at the corresponding point on the map.
  - A round black/white toggle above the chart flips it to a fully opaque
    background for reading over visually busy map areas.
- **Save as GPX** — exports a standard GPX 1.1 file (`<trk>`/`<trkseg>`
  with lat/lon/ele per point), named from the "Track name" field.
- **Mobile-optimized layout** (phone-width screens only — desktop is
  unaffected): the control panel becomes a compact floating strip
  anchored top-left, roughly 40% of the screen width, with the map
  filling the entire screen behind it. The panel itself stops right after
  the live stats; everything else (slope-smoothing setting, action
  buttons, points list) lives behind a drag-handle drawer you tap or pull
  down to reveal, so the map stays maximally visible until you actually
  need those controls.

## Usage

1. Open the app (locally or via the live link above).
2. Pick a basemap and search or pan/zoom to your area of interest, or
   load a previously saved GPX file to continue editing it.
3. Click the map to place points along your route.
4. Drag points to adjust, click a point or its row in the sidebar to
   remove it, or click the trail line to insert a new point.
5. Watch the elevation/slope profile build up automatically in the
   bottom-right corner; adjust the slope-smoothing minimum if needed.
6. Set a track name and click **Save GPX File** to download.

On a phone, tap or drag the small handle bar below the stats to reveal
the rest of the controls (they're hidden by default to keep the map
visible).

## Data sources

- Map tiles: [OpenTopoMap](https://opentopomap.org) / [OpenStreetMap](https://www.openstreetmap.org) / Google
- Elevation: [Open-Elevation](https://open-elevation.com) (primary),
  [Open-Topo-Data](https://www.opentopodata.org) SRTM90m (fallback)
- Geocoding: [Nominatim](https://nominatim.org)
- Mapping library: [Leaflet](https://leafletjs.com)

## Notes

- This is a personal utility, deployed as a static single HTML file — no
  build process, package manager, or server required.
- `index.html` is a small redirect to the current versioned app file
  (e.g. `GPX-Trail-Planner_v1.23.html`); update that redirect target when
  uploading a new version rather than replacing `index.html` itself.
- Map tiles and elevation/geocoding APIs are called directly from the
  browser at runtime, so an internet connection is required while using
  the app (not just while downloading it).
- Elevation values come from raw SRTM data (~90m grid resolution), which
  can disagree by 10-20m from what a smoothed map contour line implies at
  the same spot, and can produce noisy slope readings on closely-spaced
  points — the adjustable slope-smoothing minimum exists specifically to
  reduce that effect.
