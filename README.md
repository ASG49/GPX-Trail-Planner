# GPX Trail Planner

A single-file, no-build-step web app for plotting a hiking/trail route from
scratch by clicking points on a map, then exporting it as a standard GPX
file with altitude for every point. Runs entirely client-side in Chrome (or
any modern browser) — open `index.html` locally or host it as-is on GitHub
Pages.

**Current version: v1.17**

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
- **Location search** — jump to any place or address via OpenStreetMap's
  Nominatim geocoder.
- **Center on my location** — a round button under the zoom control
  centers the map on the browser's current geolocation; the map also
  tries to do this automatically on load.
- **Live stats** — running point count and total distance (mi/km).
- **Altitude vs. distance profile** — a semi-transparent chart overlay,
  bottom-right of the map, rendered as inline SVG (no chart library):
  - Cubic-spline (Catmull-Rom) curves for both series, so the lines pass
    smoothly through every data point.
  - A secondary Y2 axis for slope %, computed by looking ahead until each
    segment covers at least ~75m of real distance — this avoids spurious
    spikes from elevation-grid noise on closely-spaced points.
  - Solid dots at each real altitude point on the elevation curve.
  - Two-way hover linkage: hovering a map marker draws a red line on the
    chart at that point's distance; hovering a chart altitude dot shows a
    red arrow at the corresponding point on the map.
  - A round black/white toggle above the chart flips it to a fully opaque
    background for reading over visually busy map areas.
- **Save as GPX** — exports a standard GPX 1.1 file (`<trk>`/`<trkseg>`
  with lat/lon/ele per point), named from the "Track name" field.

## Usage

1. Open `index.html` in a browser (or visit the GitHub Pages URL once
   hosted).
2. Pick a basemap and search or pan/zoom to your area of interest.
3. Click the map to place points along your route.
4. Drag points to adjust, click a point or its row in the sidebar to
   remove it, or click the trail line to insert a new point.
5. Watch the elevation/slope profile build up automatically in the
   bottom-right corner.
6. Set a track name and click **Save GPX File** to download.

## Data sources

- Map tiles: [OpenTopoMap](https://opentopomap.org) / [OpenStreetMap](https://www.openstreetmap.org) / Google
- Elevation: [Open-Elevation](https://open-elevation.com) (primary),
  [Open-Topo-Data](https://www.opentopodata.org) SRTM90m (fallback)
- Geocoding: [Nominatim](https://nominatim.org)
- Mapping library: [Leaflet](https://leafletjs.com)

## Notes

- This is a personal utility, deployed as a static single HTML file — no
  build process, package manager, or server required.
- Map tiles and elevation/geocoding APIs are called directly from the
  browser at runtime, so an internet connection is required while using
  the app (not just while downloading it).
