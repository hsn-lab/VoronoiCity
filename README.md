# VoronoiCity
A single-file, offline interactive tool that answers one question for every point on Earth: which of four chosen cities is geographically nearest?

# What it does
You type in four city names (autocomplete suggests from a built-in list of roughly 200 major cities worldwide).
The tool samples a dense grid of latitude/longitude points across the globe (1200 x 600, so 720,000 points).
For every grid point, it calculates the great-circle distance to each of the four cities using the haversine formula, and records which city is closest.
Each point is coloured according to its nearest city, producing four coloured regions on an equirectangular map, plus a graticule (lat/lon grid lines) and labelled markers for the four cities.
Why haversine, not straight-line distance
A flat map distorts real-world distances, especially near the poles. The haversine formula calculates distance along the curved surface of a sphere, so the regions it produces reflect genuine "closest city" boundaries, not an artefact of the map projection. This is also why the boundaries curve rather than forming straight lines, even though each boundary is technically a great-circle arc (the shortest path between two points on a sphere).

# Why it's offline
Live geocoding (turning a typed city name into coordinates via an API) needs an external network call. The sandbox this tool runs in cannot make arbitrary outbound requests, so there's no way to hit a geocoding service from inside the page itself.

Instead, the tool ships with a built-in dictionary of about 200 major world cities and their coordinates, hardcoded directly in the script. Typing a name that matches one of these (case-insensitive) works instantly, with no network dependency at all, which also means the tool keeps working if it's shared or saved offline.

The trade-off: obscure towns not in the list won't be recognised. The fix, if you need it, is to add more "City Name":[lat,lon] entries to the CITIES object in the script.

# How the map is drawn
The projection is simple equirectangular (longitude mapped linearly to x, latitude to y), not the Robinson-style curved map in the original Reddit image. This keeps the code self-contained and avoids needing an external coastline dataset, which the sandbox also can't fetch.
There are no coastlines drawn. The graticule lines (every 30 degrees) are there to help you orient the colour regions against where the continents would be.
Each of the four cities gets a fixed colour (blue, orange, pink, green) shown in the input labels, on the map markers, and in the legend underneath.
Using it
Default cities are the four from the original image's main regions (Edinburgh, Aberdeen, Glasgow, Inverness) so you can see the comparison immediately.
Change any of the four fields and click "Generate map" to redraw.
If a typed name isn't recognised, the tool tells you and leaves the map as it was; it won't silently guess.
Limitations
Exactly four cities only, not a variable number.
No real coastlines, just the region colouring and a reference grid.
The city list, while broad, is not exhaustive. Very small or lesser-known towns may need to be added manually.
Resolution is capped at 1200x600 pixels for performance; this is more than enough to see clean boundaries but isn't infinitely zoomable.

# Inspired by
<img width="1288" height="948" alt="image" src="https://github.com/user-attachments/assets/6f7568ab-4c0d-4a8a-9234-6f363b9a1e8e" />
https://www.reddit.com/r/Scotland/comments/1wkt11h/the_world_by_closest_scottish_city/

<img width="1080" height="567" alt="image" src="https://github.com/user-attachments/assets/629e9c51-e89a-4a74-82b4-7375ef5ab860" />
https://www.reddit.com/r/Wales/comments/1wt5uq0/the_world_by_closest_welsh_city/
