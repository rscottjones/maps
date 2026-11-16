# Reverse-order test posts

Two posts containing the same five shortcodes in opposite orders. Every
order-dependence bug this project has had would have been caught by the
second one.

Copy `content/` and `data/` into a blog that has Maps, Tracks,
Travelpath and Colormaps installed, upload any GPX file as
`/uploads/demo.gpx`, publish, and check both posts. Both must show:

- no errors in the browser console
- clustering on the Maps map and on the Japan Travelpath map
- one `<script>` tag per library (Leaflet, markercluster, leaflet-gpx,
  geodesic, polyline-decorator)
- one photo lightbox: click Porto, then arrow to Sintra
- the same maps in both posts

The data exercises the paths that have broken before: a photo pin, a
comma-separated gallery pin, a video pin, an audio pin, an `fa-` icon,
a numbered icon, an emoji icon, the four badge icons that used to crash
(`camera`, `video`, `image`, `gallery`), waypoints, and great-circle
routes with chevrons.
