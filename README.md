# Fieldnotes Timeline

A local, browser based Google Timeline analyzer. Open `index.html` in a modern browser; no server or build step is required. Select a JSON file exported from Google Takeout.

## What it does

- Reads Google Timeline `semanticSegments` / `timelineObjects`, Semantic Location History visit files, and legacy `Records.json` location points.
- Includes visits whose duration is strictly greater than the selected minimum (30 minutes by default).
- Filters by inclusive start/end date, location, and minimum visit duration; all summaries, table values, histogram observations, and CSV export follow the active filters.
- Merges daily location columns within a configurable distance radius (1 mile by default), after totaling each original location by date. Changing the radius rebuilds the grouped table, map, breadcrumbs, histogram, and CSV data. Locations without coordinates remain separate; groups are pairwise bounded so a chain of nearby stops cannot grow beyond the selected radius.
- Automatically labels coordinate groups using Photon reverse geocoding on OpenStreetMap data: it prefers a nearby named business or place, then falls back to an address or road name. Results are cached in browser storage. You can enter a custom label to override a match or open the optional Google Maps search link.
- Shows a map and chronological visit breadcrumbs that follow the same active filters as the table and histogram. Street map is the default; satellite and satellite with road labels are also available. The selected background is remembered in browser storage.
- Remembers the last selected file handle locally when the browser supports the File System Access API, and reloads it automatically when file access is still granted. Otherwise, use “Reload last file” to grant access again or choose the file manually.
- Groups each included visit under its local start date and location, summing durations into a date-by-location table.
- Draws a histogram of the nonzero daily location totals, in hours. Adjust the number of histogram bins.
- Exports the daily table as CSV.

Timeline files are processed in the browser and are not uploaded. To create automatic place labels, the app sends grouped GPS center coordinates one at a time to Photon, which uses OpenStreetMap data; the returned labels and custom place labels stay in browser storage. Photon is a public demo service with no availability guarantee, so some locations may remain unlabeled if it is unavailable or has no nearby mapped place. Map backgrounds load third-party tiles for the displayed area: OpenStreetMap for streets and Esri for satellite imagery and road labels. Tile requests disclose the map area being viewed. Dashed connectors indicate visit order, not the actual route traveled. Semantic visit segment times are used directly. For legacy GPS point records, stays are approximated by grouping consecutive points within 150 meters and at most 20 minutes apart; the resulting elapsed span can be less reliable than semantic visits, especially when GPS samples are sparse. GPS-only location groups are automatically matched to nearby OpenStreetMap businesses or addresses when possible; unmatched groups retain the coordinate prompt.

The table assigns a qualifying visit's full duration to the date on which it starts. Dates follow the browser's local timezone.
