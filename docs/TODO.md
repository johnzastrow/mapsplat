# MapSplat TODO List

**Version:** 0.6.5
**Last Updated:** 2026-03-03

---

## Priority Legend

- 🔴 **Critical** - Blocks basic functionality
- 🟠 **High** - Important for usability
- 🟡 **Medium** - Nice to have
- 🟢 **Low** - Future enhancement

---

## Open Items

### Critical

- [x] 🔴 **Bundle MapLibre GL JS assets for offline use** ✅ v0.6.6
  - New Offline tab; downloads maplibre-gl.js/css and pmtiles.js at export time
  - Falls back to CDN with warning if download fails

### High

- [ ] 🟠 **Handle null category values in categorized renderer**
  - Categorized renderer can have a NULL category
  - MapLibre `match` expressions handle null differently than QGIS
  - File: `style_converter.py:_convert_categorized()`

- [ ] 🟠 **Handle "all other values" catch-all category**
  - QGIS has a catch-all category for unmatched values
  - Map to the default/fallback value in MapLibre `match` expression
  - File: `style_converter.py:_convert_categorized()`

- [ ] 🟠 **Layer ordering control**
  - Currently layer order in output may not match QGIS layer panel order
  - Respect QGIS layer order in style.json layer list
  - File: `mapsplat_dockwidget.py`, `style_converter.py`

### Medium

- [ ] 🟡 **Remember last output folder**
  - Store in QSettings, restore on plugin open
  - File: `mapsplat_dockwidget.py`

- [ ] 🟡 **Add layer count to UI**
  - Show "X of Y layers selected" summary, update on selection change
  - File: `mapsplat_dockwidget.py`

- [ ] 🟡 **Validate output folder is writable**
  - Check write permissions before starting export
  - File: `mapsplat_dockwidget.py:_validate_export()`

- [ ] 🟡 **Support graduated color ramps**
  - Extract colors from `QgsGraduatedSymbolRenderer`
  - Generate MapLibre `interpolate` expression for smooth color transitions
  - File: `style_converter.py:_convert_graduated()`

- [ ] 🟡 **Support graduated size**
  - Point size or line width driven by attribute value
  - File: `style_converter.py`

- [ ] 🟡 **Add minzoom/maxzoom per layer**
  - Extract from QGIS scale-dependent visibility settings
  - Apply to MapLibre layer definition
  - File: `style_converter.py`

- [ ] 🟡 **Robust style.json import**
  - Validate imported JSON structure before merging
  - Handle malformed files gracefully with a clear error
  - File: `mapsplat_dockwidget.py:_import_style()`

- [ ] 🟡 **Legend generation**
  - Generate a proper legend panel from `style.json` layer colors/symbols
  - Currently only color swatches exist; no label-driven legend
  - File: `exporter.py:_get_html_template()`

- [ ] 🟡 **Basemap switcher in viewer**
  - Dropdown to switch between basemap styles at runtime
  - Remember selection in `localStorage`
  - File: `exporter.py:_get_html_template()`

### Low

- [ ] 🟢 **Raster layer export**
  - Export raster layers to PMTiles via `gdal_translate`
  - Rasters placed below vector layers in style.json
  - File: `exporter.py`, `mapsplat_dockwidget.py`

- [ ] 🟢 **External basemap URL (non-Protomaps)**
  - Support OSM, Stadia, MapTiler XYZ tile URLs as basemap
  - Currently basemap overlay only supports Protomaps PMTiles format
  - File: `mapsplat_dockwidget.py`

- [ ] 🟢 **Share/embed code**
  - Generate iframe embed snippet
  - Copy-to-clipboard button in viewer
  - File: `exporter.py:_get_html_template()`

- [ ] 🟢 **Direct cloud upload**
  - Upload output folder directly to AWS S3 / Cloudflare R2 / SFTP
  - File: new `uploader.py`, `mapsplat_dockwidget.py`

- [ ] 🟢 **Preview in plugin before export**
  - Show a small embedded map preview of the current project
  - Complex — requires embedded browser widget

---

## Testing

- [ ] 🟠 **Expand unit tests for style_converter.py**
  - Categorized null/catch-all handling
  - Graduated color ramp expression generation
  - File: `test/test_style_converter.py`

- [ ] 🟠 **Integration test with sample data**
  - Create a test GeoPackage with known features and styles
  - Run full export, validate output directory structure and style.json
  - File: `test/test_exporter.py`

---

## Completed ✅

### Core
- [x] 🔴 Validate ogr2ogr PMTiles generation (GDAL 3.8+) — v0.1.0
- [x] 🟠 GDAL version check and PMTiles driver availability warning — v0.1.7
- [x] 🟠 Cancel button to abort long-running exports — v0.1.7
- [x] 🟡 Max zoom spinbox (4–18, default 6) — v0.1.7
- [x] 🟠 serve.py with HTTP Range request support — v0.1.7
- [x] serve.py --port and --no-browser flags — v0.6.5

### Symbology
- [x] 🟠 Single Symbol renderer (fill, line, marker) — v0.1.0
- [x] 🟠 Categorized renderer — v0.1.0
- [x] 🟠 Graduated renderer — v0.1.0
- [x] 🟠 Opacity extraction (fill, line, circle, stroke) — v0.2.0
- [x] 🟠 Line width unit conversion (mm → px) — v0.2.0
- [x] 🟠 Line dash patterns, cap/join styles — v0.2.0
- [x] 🟠 Multiple symbol layers per renderer — v0.2.0
- [x] 🟠 Labels (text field, font, size, color, halo, placement) — v0.2.0
- [x] 🟠 Rule-based renderer with filter expression conversion — v0.2.0
- [x] 🟠 SVG marker → sprite atlas export — v0.4.0

### Multi-layer & Options
- [x] 🟠 Separate PMTiles per layer mode — v0.1.9
- [x] 🟡 Layer visibility toggles in HTML viewer — v0.1.8
- [x] 🟡 Legend color swatches in layer panel — v0.1.8

### Viewer
- [x] 🟠 Tabbed dockwidget (Export / Viewer / Log tabs) — v0.5.0
- [x] 🟡 7 configurable viewer controls (scale bar, geolocate, fullscreen, coords, zoom, reset-view, north-reset) — v0.5.2
- [x] 🟡 Embeddable HTML with BEGIN/END copy-paste markers — v0.6.1

### Basemap
- [x] 🟠 Basemap overlay mode (Protomaps PMTiles, local or URL) — v0.3.0
- [x] 🟠 Basemap clipping to data extent via pmtiles CLI — v0.3.0
- [x] 🟠 Basemap + business layer style merge — v0.3.0

### Config & Logging
- [x] 🟠 Export log to file with timestamps and log levels — v0.5.1
- [x] 🟠 Config save/load (TOML, human-editable) — v0.6.0

### Style Roundtripping
- [x] 🟡 Export style.json + re-import from Maputnik — v0.2.2
- [x] 🟡 Style-only export (skip data, regenerate HTML/style) — v0.2.1

### Documentation
- [x] PLAN.md, TODO.md, CHANGELOG.md, README.md, REQUIREMENTS.md
- [x] README table of contents — v0.6.5
- [x] Deployment guides (GitHub Pages, Netlify, S3, Nginx, Caddy) — v0.6.4
