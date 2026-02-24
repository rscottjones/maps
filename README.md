# Maps for Micro.blog

An interactive map plugin for [Micro.blog](https://micro.blog) that lets you place pins on one or more maps using simple YAML data files. Built on [Leaflet.js](https://leafletjs.com/) and [OpenStreetMap](https://www.openstreetmap.org/).

Inspired by [microblog-map](https://github.com/vladcampos/microblog-map) by Vlad Campos.

## Example uses

- You travel often around Europe, and want a pin for each country that links to the category archive of posts you have for each place.
- You post about your day’s hike and want to share the photos on a map using the built-in lightbox gallery and satellite map base.
- Keep track of your progress in your quest goal to visit each of the National Parks, using different pins to show which ones you’ve completed and which ones you still have remaining. Add the date of your visit to the description field.
- You create a map of places you visited on your last trip, with links to each blog post.
- You want a map showing all of the airports you’ve flown through using various plane emojis as pin markers. Use the built-in legend to show whether each airport visit was roundtrip, one way, or just a layover.
- Link to your pages displaying your photo collections using pins on a map.
- Create a map of places you’ve reviewed on Yelp with links directly to each review on Yelp’s website.
- Create a map of your summer trips using numbered pins to show which year you were there.
- You want to create a ranking of your favorite coffeeshops in your area, so you use the numbered pins to rank them on a map. Each pin links to a section further down the page where you explain why that shop earned its ranking.

## Features

- **Multiple maps** — Create as many maps as you want, each powered by its own data file.
- **120+ built-in icons** — Standalone shapes, circle badges, and numbered circles. Mix and match on the same map.
- **Emoji support** — Use any emoji or unicode character as a marker.
- **Per-pin colors** — Set a color on any individual pin using CSS color names or hex values.
- **Custom defaults** — Override the default pin color and icon shape per map via shortcode parameters.
- **Optional links** — Pins can link to a post or category page. Links open in a new tab.
- **Built-in lightbox** — If a pin's `url` points to an image file, clicking it opens the image in a full-screen overlay. When a map has multiple image pins, arrows and swipe gestures let you browse through all of them.
- **Descriptions** — Add a short text description to any pin, displayed below the name in the popup.
- **Legend** — Optionally display a key below the map explaining what each icon and color means.
- **Self-contained** — Works with any Micro.blog theme (mnml, Tiny, etc.) with no custom CSS, header partials, or microhooks required.

## Installation

1. Copy `layouts/shortcodes/map.html` into your blog's custom theme at `layouts/shortcodes/map.html`.
2. Create one or more YAML data files in your `data/` folder (e.g., `data/europe.yml`). See [Data File Format](#data-file-format) below.
3. Add the shortcode to any post or page. See [Usage](#usage) below.

After installation your custom theme should include:

```
layouts/
  shortcodes/
    map.html
data/
  europe.yml              ← your pin data
  europe-legend.yml       ← optional legend
```

## Usage

### Basic

Add a map to any post or page by referencing the name of your data file (without the `.yml` extension):

```
{{< map "europe" >}}
```

This renders a map using the pins defined in `data/europe.yml`.

### Multiple maps

You can place as many maps as you want on a single page, each referencing a different data file:

```
{{< map "europe" >}}
{{< map "south-america" >}}
```

### Optional parameters

To customize a map's appearance, switch to named parameters:

```
{{< map source="europe" height="700px" >}}
{{< map source="europe" color="Tomato" >}}
{{< map source="europe" color="#8B5CF6" >}}
{{< map source="europe" icon="circle-dot" >}}
{{< map source="europe" basemap="positron" >}}
{{< map source="europe" legend="europe-legend" >}}
```

| Parameter | Default | Description |
|---|---|---|
| `source` | *(required)* | Name of the YAML data file in `data/` (without `.yml`). |
| `height` | `500px` | Height of the map container. Any valid CSS value works. |
| `color` | `#4682B4` (SteelBlue) | Default pin color for pins that don't specify their own. Accepts CSS color names or hex values. |
| `icon` | `pin` | Default icon shape for pins that don't specify their own. Accepts any value from [Available Icons](#available-icons). |
| `basemap` | `voyager` | Base map tile style. Options: `osm`, `voyager`, `positron`, `dark`, `topo`, `satellite`. See [Base Maps](#base-maps). |
| `legend` | *(none)* | Name of a legend YAML file in `data/` (without `.yml`). When provided, a key is displayed below the map. |

## Data File Format

Each data file is a YAML list stored in your `data/` folder. Only `name`, `lat`, and `lon` are required — everything else is optional.

```yaml
- name: "Porto, Portugal"
  lat: 41.1579
  lon: -8.6291
  url: "/categories/porto"
  color: "Green"
  icon: "circle-check"
  description: "Three days exploring the Ribeira district and port wine cellars."

- name: "Paris, France"
  lat: 48.8566
  lon: 2.3522
  icon: "star"
  color: "Gold"

- name: "Tokyo, Japan"
  lat: 35.6762
  lon: 139.6503
  icon: "🏯"

- name: "Barcelona, Spain"
  lat: 41.3874
  lon: 2.1686
  url: "/categories/barcelona"

- name: "Berlin, Germany"
  lat: 52.5200
  lon: 13.4050
```

### Field reference

| Field | Required | Description |
|---|---|---|
| `name` | Yes | The label displayed in the pin's popup. |
| `lat` | Yes | Latitude coordinate. |
| `lon` | Yes | Longitude coordinate. |
| `url` | No | A link destination. The pin's name becomes clickable and opens in a new tab. If the URL points to an image file (`.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`), clicking it opens a lightbox overlay instead, with arrows to browse all image pins on the map. |
| `description` | No | A short plain text description displayed below the name in the popup. |
| `color` | No | Color for this pin. Accepts any CSS color name (`Green`, `Tomato`, `Gold`) or hex value (`#16A34A`). When omitted, the pin uses the map's default color. See [CSS color names](https://www.w3schools.com/colors/colors_names.asp) for a full list. |
| `icon` | No | Icon shape for this pin. Defaults to `pin`. See [Available Icons](#available-icons). |

## Legend

To display a key below your map, create a separate YAML file for the legend and reference it with the `legend` parameter. The legend is entirely optional — maps without it display with fully rounded corners as usual.

When a legend is present, the map's bottom corners become square and the legend bar attaches flush underneath with its own rounded bottom corners, so they read as one unit.

### Legend file format

```yaml
- label: "Visited"
  icon: "circle-check"
  color: "Green"
  url: "#visited"

- label: "Favorite"
  icon: "star"
  color: "Gold"

- label: "Photo spot"
  icon: "camera"
  color: "Tomato"

- label: "Wishlist"
  icon: "circle-dot"
```

### Legend field reference

| Field | Required | Description |
|---|---|---|
| `label` | Yes | The text displayed next to the icon. |
| `icon` | No | The icon shape to display. Defaults to `pin` if omitted. Supports all the same values as the pin `icon` field, including emoji. |
| `color` | No | The color of the icon. When omitted, uses the map's default color. |
| `url` | No | A link destination. When present, the label becomes a clickable link. Useful for linking to a bookmark or section further down the page (e.g., `#visited`). |

### Shortcode usage

```
{{< map source="europe" legend="europe-legend" >}}
```

## Available Icons

### Standalone shapes

| Value | Description |
|---|---|
| `pin` | Classic teardrop map pin with a white center dot *(default)* |
| `pin-small` | Smaller version of the standard pin — useful for crowded maps |
| `star` | Five-pointed star |
| `heart` | Heart shape |
| `point` | Small filled dot |
| `camera` | Camera silhouette with a white lens circle |
| `video` | Video camera silhouette |
| `image` | Photo frame with a white sun and landscape |
| `gallery` | Stacked photo frames with a white sun and landscape |

### Circle badges

Filled circles with a white symbol cut out inside.

| Value | Description |
|---|---|
| `circle-check` | Checkmark |
| `circle-x` | X mark |
| `circle-heart` | Heart |
| `circle-star` | Star |
| `circle-dot` | Dot |
| `circle-new` | The word "NEW" |

### Numbered circles

Filled circles with a white number inside. Available in two styles:

**Single digit:** `circle-0` through `circle-9` — larger text, good for short sequences.

**Two-digit (zero-padded):** `circle-00` through `circle-99` — slightly smaller text to fit two characters. Use these for years (`circle-24` for 2024, `circle-85` for 1985), numbered routes, or longer sequences.

```yaml
icon: "circle-3"
icon: "circle-07"
icon: "circle-99"
```

Use the [legend](#legend) to explain what the numbers represent — a stop number on a walking tour, a year visited, etc.

### Emoji / Unicode

Any `icon` value that doesn't match a built-in name is rendered directly as an emoji or unicode character:

```yaml
icon: "🏯"
icon: "🍕"
icon: "⛪"
icon: "🏖️"
```

Emoji appearance varies across operating systems and browsers. The `color` field has no effect on emoji icons.

All built-in icons respect the `color` field. For example, a `circle-check` with `color: "Green"` renders as a green circle with a white checkmark.

## Base Maps

By default, maps use CARTO Voyager tiles. You can switch to a different style using the `basemap` parameter:

```
{{< map source="europe" basemap="positron" >}}
```

| Value | Description |
|---|---|
| `osm` | Standard OpenStreetMap tiles |
| `voyager` | CARTO Voyager — clean, colorful street map with a modern feel *(default)* |
| `positron` | CARTO Positron — light gray, minimal. Great for making pins stand out |
| `dark` | CARTO Dark Matter — dark gray. Striking with colorful pins |
| `topo` | OpenTopoMap — topographic map with contour lines and elevation shading |
| `satellite` | Esri World Imagery — satellite photography |

All options are free and require no API key. The Esri satellite tiles use a legacy service that may require registration in the future; see [Esri's terms](https://wiki.openstreetmap.org/wiki/Esri) for details.

## Starter Data

See the Starter Data folder for yaml files to get you started with a number of possible travel quests. These were generated by ChatGPT, Claude, or Gemini and may contain errors or omissions, but are a helpful starting point.

## Tips

- **Finding coordinates:** Search for a location on [Google Maps](https://maps.google.com), right-click the spot, and the latitude/longitude will appear at the top of the context menu. Click to copy.
- **Scroll zoom is disabled** so visitors don't get trapped in the map while scrolling. They can still zoom with the `+`/`-` buttons or by pinching on mobile.
- **Auto-fit:** The map automatically zooms and pans to fit all your pins.
- **Mixing icons and colors:** Combine different icons and colors to represent categories — `circle-check` in green for visited places, `star` in gold for favorites, `circle-dot` for your wishlist. Use a legend to explain the key.
- **Photo maps:** Set each pin's `url` to an image file. The lightbox handles the rest — click a pin name to view the photo, then use arrows, keyboard, or swipe to browse all photos on the map. Click the backdrop or press Escape to close. On Micro.blog, link to the `-m` (medium) version of an image to keep load times fast.

## Theme Compatibility

The shortcode loads Leaflet's CSS and JavaScript on demand — no changes to your theme's `<head>` or footer are needed. It includes CSS overrides that prevent common theme styles (such as `max-width` or `border-radius` on images) from shifting Leaflet's map tiles and pin positions. This is a [known issue](https://rsjon.es/2026/01/31/fixing-pin-locations-using-microblog/) with some Micro.blog themes, particularly mnml.

## Possible future features

- marker clustering for large datasets
- support font awesome icons

## Credits

- [Leaflet.js](https://leafletjs.com/) for the mapping library
- [OpenStreetMap](https://www.openstreetmap.org/) for the tile data
- [microblog-map](https://github.com/vladcampos/microblog-map) by Vlad Campos for the original inspiration
- Pin position fix documented by [R. Scott Jones](https://rsjon.es/2026/01/31/fixing-pin-locations-using-microblog/)

## License

MIT
