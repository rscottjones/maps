# Maps

A [Micro.blog](https://micro.blog) plugin that adds interactive maps to any post. Drop a shortcode into your post, point it at a data file, and you get a fully rendered Leaflet map — pins, popups, legends, clustering, lightbox, video, and audio, all loaded on demand with no theme configuration required.

---

## Installation

Install the plugin from the Micro.blog plugin directory. No head partials, footer partials, or microhooks are needed. All CSS and JavaScript is injected automatically, and only on pages that actually contain a map.

---

## Quick start

Create a data file at `data/maps/europe.yml`:

```yaml
- name: "Lisbon, Portugal"
  lat: 38.7169
  lon: -9.1399

- name: "Porto, Portugal"
  lat: 41.1579
  lon: -8.6291
  url: "/categories/porto"
  color: "Green"
  icon: "check"
  note: "Three days exploring the Ribeira district."
```

Add the shortcode to a post:

```
{{< map "europe" >}}
```

That's it. The map loads Leaflet on demand, fits the bounds to your pins, and handles everything else automatically.

---

## Shortcode parameters

All parameters except `data` are optional.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `data` | *(required)* | Name of the YAML file in `data/maps/` (without the `.yml` extension). Can also be passed as the first positional argument. |
| `height` | `500px` | Height of the map container. Accepts any CSS value (`400px`, `60vh`). A bare number is treated as pixels. |
| `color` | `#4682B4` | Default pin color for locations that don't specify their own. Accepts any CSS color value or named color. |
| `icon` | `pin` | Default icon shape for locations that don't specify their own. See [Icons](#icons) for all options. |
| `basemap` | `osm` | Map tile style. See [Basemaps](#basemaps) for all options. |
| `legend` | — | Name of a legend YAML file in `data/maps/`. Renders a legend bar beneath the map. |
| `cluster` | — | Set to `"yes"` to group nearby pins into count bubbles. |

### Examples

```
{{< map "europe" >}}
{{< map "south-america" height="700px" >}}
{{< map "trips" color="#E11D48" icon="dot" >}}
{{< map "national-parks" basemap="topo" legend="parks-legend" >}}
{{< map "cities" cluster="yes" >}}
```

---

## Data file format

Each entry in the YAML file becomes one pin on the map.

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Label shown in the pin's popup. |
| `lat` | Yes | Latitude as a decimal number. |
| `lon` | Yes | Longitude as a decimal number. |
| `url` | No | Link attached to the pin name in the popup. See [URL types](#url-types) for special behaviors. |
| `color` | No | Pin color for this location. Overrides the shortcode default. |
| `icon` | No | Icon shape for this location. Overrides the shortcode default. |
| `note` | No | A short note displayed beneath the pin name in the popup. |

### URL types

The `url` field has special behavior depending on what it points to:

- **Image** (`.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`) — clicking the pin name opens a fullscreen lightbox. Multiple image pins on the same map support gallery navigation with arrows, keyboard arrow keys, and swipe gestures.
- **Video** (`.mp4`, `.m4v`, `.mov`, `.webm`, `.ogv`) — clicking opens a video overlay with playback controls. The video autoplays muted.
- **HLS stream** (`.m3u8`) — same as video. Safari plays natively; other browsers load [hls.js](https://github.com/video-dev/hls.js/) on demand.
- **Audio** (`.mp3`, `.m4a`, `.aac`, `.ogg`, `.oga`, `.wav`, `.flac`) — an inline audio player is embedded directly in the popup beneath the pin name.
- **Anything else** — opens in a new tab.

---

## Legend file format

Legend files live in the same `data/maps/` directory as map data files. Each entry becomes one item in the legend bar.

| Field | Required | Description |
|-------|----------|-------------|
| `label` | Yes | Text displayed next to the icon. |
| `icon` | No | Icon shape for this legend entry. Defaults to the shortcode's `icon` parameter. |
| `color` | No | Color for this legend entry. Defaults to the shortcode's `color` parameter. |
| `url` | No | Makes the label a link. |

```yaml
- label: "Visited"
  icon: "check"
  color: "Green"
  url: "/categories/visited"

- label: "Wishlist"
  icon: "pin"
  color: "#aaa"
```

The legend bar is rendered directly below the map with a matching border radius.

---

## Icons

Maps, Tracks and Travelpath draw from one icon set, defined in `geo-core-v1.html`. Any name below works anywhere an `icon:` field is accepted.

| Category | Values |
|---|---|
| Pins | `pin`, `pin-small` |
| Plain shapes | `point` (12 px), `circle` (20 px, white stroke), `disc` (26 px), `bullseye` (hollow ring) |
| Status badges | `check`, `x`, `dot`, `plus`, `minus` |
| Favourites | `heart`, `star`, `star-half`, `star-empty` |
| Media | `camera`, `video`, `image`, `gallery` |
| Places | `mountain`, `binoculars`, `landmark`, `utensils`, `droplet`, `microphone`, `flag`, `warning` |
| Endpoints | `start`, `finish` (Tracks places these automatically) |
| Labels | `icon-new` |
| Numbered | `icon-0` … `icon-9` (large single digit), `icon-00` … `icon-99` (small two digit) |
| Font Awesome | any `fa-` prefixed name, e.g. `fa-utensils`, `fa-mug-hot` |
| Emoji | any other value, rendered as-is, e.g. `"🏯"`, `"⛺"` |

Badges are a white glyph inside a filled circle, coloured by the marker's `color` field. `fa-` icons render a coloured glyph on a transparent background instead; the Font Awesome stylesheet loads only on pages that use one.

### Emoji and Unicode

Any value that isn't a recognized shape name or `fa-` prefix is rendered as-is, centered on the pin location:

```yaml
icon: "🏯"
icon: "🍕"
icon: "★"
```

---

## Basemaps

| Value | Source | Notes |
|-------|--------|-------|
| `osm` | OpenStreetMap | Default |
| `voyager` | Carto Voyager | Colorful, detailed |
| `positron` | Carto Positron | Minimal light style |
| `topo` | OpenTopoMap | Topographic contours |
| `cycle` | CyclOSM | Cycling-focused |
| `satellite` | Esri World Imagery | Aerial photography |
| `streets` | Esri World Street Map | |
| `esri-topo` | Esri World Topo | |
| `esri-relief` | Esri Shaded Relief | |
| `natgeo` | Esri National Geographic | |
| `usgs-topo` | USGS Topo | United States only |
| `usgs-hybrid` | USGS Imagery + Topo | United States only |

---

## Clustering

Setting `cluster="yes"` groups nearby pins into count bubbles at low zoom levels. Clicking a cluster zooms the map to fit its members. At maximum zoom, overlapping pins spread out into a spiderweb pattern so each one is individually clickable.

The [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) library is loaded on demand only on pages where clustering is enabled.

---

## Multiple maps on one page

Multiple `{{< map >}}` shortcodes on the same page work correctly. The shared CSS and JavaScript (styles, icon shapes, basemap definitions, media helpers) are emitted only once regardless of how many maps appear. Each map gets its own instance script with its own data, ID, and configuration.

Leaflet itself is also loaded only once — subsequent maps queue up and initialize as soon as the first map's Leaflet load completes.

---

## Dark mode

The plugin includes dark mode styles for popups, zoom controls, attribution, the noscript fallback, cluster bubbles, and legend bars. These activate automatically via `prefers-color-scheme: dark` with no configuration needed.

---

## Accessibility

A visually hidden paragraph is emitted before each map for RSS readers and screen readers:

> *[Interactive map — visit the original post to explore it.]*

This is visible in plain-text environments where CSS is stripped (such as many RSS readers) but hidden on screen via `.map-sr-only`. The map container also carries `role="application"` and an `aria-label`.

---

## Using Maps with Tracks and Travelpath

The three plugins are designed to share a page. Each ships an identical copy of `geo-core-v1.html` (icons, basemaps, the library loader, the lightbox and video overlays) and `geo-media-styles-v1.html`. Whichever shortcode renders first emits them once; the others skip. Leaflet and every other library load once per page, whatever order the shortcodes appear in.

The version number is part of those file names on purpose: Micro.blog uses only one copy of a file when two plugins ship the same path, so a versioned name guarantees each plugin runs with the core it was released with.

---

## Theming notes

The plugin ships with CSS overrides that correct common theme interference with Leaflet. In particular, themes like mnml apply `max-width`, `border-radius`, or `margin` to `img` tags inside `article` elements, which shifts pin icons and map tiles. These are neutralized with targeted `!important` rules scoped to `.leaflet-container`, which live in the shared core. The plugin's own classes are all prefixed `mb-map-`.

---

## File structure

```
plugin.json
layouts/
  shortcodes/
    map.html                ← shortcode: parameter resolution, Scratch guards, per-map script
  partials/
    map-styles.html           ← Maps-only CSS (container, legend, cluster bubbles)
    geo-core-v1.html          ← shared with Tracks and Travelpath: icons, basemaps, loader, overlays
    geo-media-styles-v1.html  ← shared: lightbox, video overlay, audio player CSS
test/                         ← two reverse-order test posts and their data
```

The three partials are called via `partialCached`, so Hugo evaluates each one once per build and reuses the result. CSS and JavaScript are emitted only on pages that contain at least one `{{< map >}}` shortcode.
