# H3 Tutorial: Intro to h3-js (v4)
![H3 Grid](https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/9758ff41bb283608b33d3b864a5a576d70e8229e/grid_tan.png)
The h3-js library is a JavaScript version of [H3](https://uber.github.io/h3), Uber's hexagon-based geospatial indexing system. The code is transpiled from C using [emscripten](https://emscripten.org/). This intro provides a quick overview of common functions in h3-js v4, which changed most function names in the API.
[github.com/uber/h3-js](https://github.com/uber/h3-js)
## Core Functions
The core of the h3-js library are the functions that provide the H3 index for geographic coordinates and vice versa. Finding an H3 index requires the caller to specify the grid resolution, which determines the size of the hexagon. The higher the resolution of the grid, the smaller the hexagons, from res 0 (continental) to res 15 (1 square meter). Res 9 is roughly a city block.
```js
// Index a lat/lng point to an H3 index
h3.latLngToCell(20, 123, 2)
```
```js
// Get the center of an H3 index as a lat/lng point
h3.cellToLatLng("8928342e20fffff")
```
```js
// Get the vertices of an H3 index as lat/lng points.
// Passing "true" as the second parameter yields GeoJSON lng/lat points and a closed loop.
h3.cellToBoundary("8928342e20fffff", true)
```
## Hierarchy
The H3 grid has a hierarchical relationship between resolutions. Each hexagon has 7 children at the next resolution, although the areas of these children are not perfectly contained in the parent. h3-js provides functions to move up and down this hierachy.
```js
// Get the children of an H3 index at the given (finer) resolution
h3.cellToChildren("8928342e20fffff", 10)
```
```js
// Get the parent of an H3 index at the given (coarser) resolution
h3.cellToParent("8928342e20fffff", 7)
```
## Traversal
A hexagon-based grid lends itself well to traversal, as each cell has 6 unambiguous neighbors (or 5, for the 12 pentagons at each resolution). h3-js provides functions to traverse neighbors by distance from some origin index.
```js
// Find all H3 indexes within a given distance from the origin (filled ring)
h3.gridDisk("8728342e2ffffff", 1)
```
```js
// Find all H3 indexes within a given distance from the origin, grouped into hollow rings
// from innermost to outermost
h3.gridDiskDistances("8728342e2ffffff", 3)
```
```js
// Get the distance in grid cells between two H3 indexes
h3.gridDistance("85283473fffffff", "8528362bfffffff")
```
```js
// Get a path of cells between two H3 indexes
h3.gridPathCells("85283473fffffff", "8528362bfffffff")
```
## Regions
The h3-js library also provides conversion functions between polygons and sets of H3 indexes, allowing users to work with arbitrarily-shaped geographic regions.
```js
// Get all H3 indexes within a geographic polygon. Inclusion is determined by whether the center
// of a given cell falls within the polygon.
h3.polygonToCells([[[37,-122], [36, -121], [36, -123]]], 5)
```
```js
// Get the multipolygon (array of polygons) for a set of H3 indexes. Indexes are expected to
// be unique and all at the same resolution
h3.cellsToMultiPolygon(["85291a6ffffffff", "85291a6bfffffff", "852834d3fffffff"])
```
## And More!
The H3 library provides a range of other functions, including utilities for inspecting H3 indexes, indexing edges between cells, and getting metadata about the grid. You can see the full API documentation on GitHub: [github.com/uber/h3-js](https://github.com/uber/h3-js)
## Dependencies
```js
h3 = require('h3-js@4.0.1')
```

Here are all 6 remaining tutorials converted to markdown:

---

## 1. H3 Tutorial: Intro to h3-js (v3)

~~~markdown
# H3 Tutorial: Intro to h3-js (v3)

![H3 Grid](https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/9758ff41bb283608b33d3b864a5a576d70e8229e/grid_tan.png)

The h3-js library is a JavaScript version of [H3](https://uber.github.io/h3), Uber's hexagon-based geospatial indexing system. **Note:** This page covers h3-js v3. For v4, see the [h3-js v4 tutorial](https://observablehq.com/@nrabinowitz/h3-tutorial-intro-to-h3-js-v4).

[github.com/uber/h3-js](https://github.com/uber/h3-js)

## Core Functions

The core of the h3-js library are the functions that provide the H3 index for geographic coordinates and vice versa. Finding an H3 index requires the caller to specify the grid resolution, which determines the size of the hexagon. The higher the resolution of the grid, the smaller the hexagons, from res 0 (continental) to res 15 (1 square meter). Res 9 is roughly a city block.

```js
// Index a lat/lng point to an H3 index
h3.geoToH3(20, 123, 2)
```

```js
// Get the center of an H3 index as a lat/lng point
h3.h3ToGeo("8928342e20fffff")
```

```js
// Get the vertices of an H3 index as lat/lng points
h3.h3ToGeoBoundary("8928342e20fffff")
```

## Hierarchy

The H3 grid has a hierarchical relationship between resolutions. Each hexagon has 7 children at the next resolution, although the areas of these children are not perfectly contained in the parent. h3-js provides functions to move up and down this hierachy.

```js
// Get the children of an H3 index at the given (finer) resolution
h3.h3ToChildren("8928342e20fffff", 10)
```

```js
// Get the parent of an H3 index at the given (coarser) resolution
h3.h3ToParent("8928342e20fffff", 7)
```

## Traversal

A hexagon-based grid lends itself well to traversal, as each cell has 6 unambiguous neighbors (or 5, for the 12 pentagons at each resolution). h3-js provides functions to traverse neighbors by distance from some origin index.

```js
// Find all H3 indexes within a given distance from the origin (filled ring)
h3.kRing("8728342e2ffffff", 1)
```

```js
// Find all H3 indexes a specific distance from the origin (hollow ring)
h3.hexRing("8728342e2ffffff", 1)
```

```js
// Get the distance in grid cells between two H3 indexes
h3.h3Distance("8728342e6ffffff", "8728342c4ffffff")
```

## Regions

The h3-js library also provides conversion functions between polygons and sets of H3 indexes, allowing users to work with arbitrarily-shaped geographic regions.

```js
// Get all H3 indexes within a geographic polygon
h3.polyfill([[[37,-122], [36, -121], [36, -123]]], 5)
```

```js
// Get the multipolygon (array of polygons) for a set of H3 indexes
h3.h3SetToMultiPolygon(["85291a6ffffffff", "85291a6bfffffff", "852834d3fffffff"])
```

## And More!

The H3 library provides a range of other functions, including utilities for inspecting H3 indexes, indexing edges between cells, and getting metadata about the grid. You can see the full API documentation on GitHub: [github.com/uber/h3-js](https://github.com/uber/h3-js)

## Dependencies

```js
h3 = require('h3-js@3.7.2')
```
~~~

---

## 2. H3 Tutorial: Rendering Hexagons

~~~markdown
# H3 Tutorial: Rendering Hexagons

There are a variety of different ways to render H3 hexagons on a map. This tutorial demonstrates a simple approach to rendering an H3 heatmap using MapboxGL.

## Data

Create dummy data as a map of h3 indexes to values between 0 and 1. There are lots of other data formats that could work here, but a map makes it easy to associate a hexagon with a value.

```js
const hexagons = (() => {
  const centerHex = h3.geoToH3(config.lat, config.lng, 8);
  const kRing = h3.kRing(centerHex, 3);
  // Reduce hexagon list to a map with random values
  return kRing.reduce((res, hexagon) => ({...res, [hexagon]: Math.random()}), {});
})();
```

## Map Rendering

Using MapboxGL, create an interactive map to show our data as a hexagon heatmap layer. The map will update appropriately when the data changes.

```js
function* map(mapboxgl, mapContainer, config, renderHexes, hexagons, renderAreas) {
  let map = this;
  if (!map) {
    map = new mapboxgl.Map({
      container: mapContainer,
      center: [config.lng, config.lat],
      zoom: config.zoom,
      style: 'mapbox://styles/mapbox/light-v9'
    });
  }

  // Wait until the map loads, then yield the container again.
  yield new Promise(resolve => {
    if (map.loaded()) resolve(map);
    else map.on('load', () => resolve(map));
  });

  renderHexes(map, hexagons);
  renderAreas(map, hexagons, 0.75);
}
```

The `renderHexes` function adds the geojson data source and associated layer to the map, updating when data or configuration changes.

```js
function renderHexes(map, hexagons) {
  // Transform the current hexagon map into a GeoJSON object
  const geojson = geojson2h3.h3SetToFeatureCollection(
    Object.keys(hexagons),
    hex => ({value: hexagons[hex]})
  );

  const sourceId = 'h3-hexes';
  const layerId = `${sourceId}-layer`;
  let source = map.getSource(sourceId);

  // Add the source and layer if we haven't created them yet
  if (!source) {
    map.addSource(sourceId, {
      type: 'geojson',
      data: geojson
    });
    map.addLayer({
      id: layerId,
      source: sourceId,
      type: 'fill',
      interactive: false,
      paint: {
        'fill-outline-color': 'rgba(0,0,0,0)',
      }
    });
    source = map.getSource(sourceId);
  }

  // Update the geojson data
  source.setData(geojson);

  // Update the layer paint properties, using the current config values
  map.setPaintProperty(layerId, 'fill-color', {
    property: 'value',
    stops: [
      [0, config.colorScale[0]],
      [0.5, config.colorScale[1]],
      [1, config.colorScale[2]]
    ]
  });
  map.setPaintProperty(layerId, 'fill-opacity', config.fillOpacity);
}
```

If we only care about values above a certain threshold, we might want to simply render those areas of the map as outlines, instead of rendering the whole heatmap. This allows us to focus on regions of interest, without obscuring the map behind the hex layer. The `renderAreas` function filters by threshold and renders the resulting outline.

```js
function renderAreas(map, hexagons, threshold) {
  // Transform the current hexagon map into a GeoJSON object
  const geojson = geojson2h3.h3SetToFeature(
    Object.keys(hexagons).filter(hex => hexagons[hex] > threshold)
  );

  const sourceId = 'h3-hex-areas';
  const layerId = `${sourceId}-layer`;
  let source = map.getSource(sourceId);

  // Add the source and layer if we haven't created them yet
  if (!source) {
    map.addSource(sourceId, {
      type: 'geojson',
      data: geojson
    });
    map.addLayer({
      id: layerId,
      source: sourceId,
      type: 'line',
      interactive: false,
      paint: {
        'line-width': 3,
        'line-color': config.colorScale[2],
      }
    });
    source = map.getSource(sourceId);
  }

  // Update the geojson data
  source.setData(geojson);
}
```

## Configuration

```js
const config = {
  lng: -122.4,
  lat: 37.7923539,
  zoom: 11.5,
  fillOpacity: 0.6,
  colorScale: ['#ffffcc', '#78c679', '#006837']
};
```

## Dependencies

- [MapboxGL](https://www.mapbox.com/mapbox-gl-js/api/) is our map-rendering library
- [h3-js](https://github.com/uber/h3-js) provides the core logic and algorithms for the hexagon grid system
- [geojson2h3](https://github.com/uber/geojson2h3) includes utilities for converting between H3 and GeoJSON

```js
mapboxgl = require('mapbox-gl@0.43.0')
h3 = require('h3-js@3.7.2')
geojson2h3 = require('https://bundle.run/geojson2h3@1.0.1')
```
~~~

---

## 3. H3 Tutorial: Using Point Layers

~~~markdown
# H3 Tutorial: Using Point Layers

Converting a point layer to H3 is quite straightforward, as binning into hexagon cells is H3's core functionality. This tutorial demonstrates how to extend basic binning with buffering operations.

## Data

Here we're loading BART station locations in GeoJSON format. Source: [bart.gov](https://www.bart.gov/schedules/developers/geo)

```js
const bartStations = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/8f1a3e30113472404feebc288e83688a6d5cf33d/bart.json'
);
```

We have several options for transforming the point layer to H3 indexes with associated values. The simplest is just to count the number of stations in each hexagon.

```js
function countPoints(geojson) {
  const layer = {};
  geojson.features.forEach(feature => {
    const [lng, lat] = feature.geometry.coordinates;
    const h3Index = h3.geoToH3(lat, lng, h3Resolution);
    layer[h3Index] = (layer[h3Index] || 0) + 1;
  });
  return normalizeLayer(layer, true);
}
```

If we're using a relatively fine resolution, however, this gives us a lot of individual hexagons. If we're interested in e.g. "areas within walking distance of a BART station", we need to buffer.

```js
function bufferPoints(geojson, radius) {
  const layer = {};
  geojson.features.forEach(feature => {
    const [lng, lat] = feature.geometry.coordinates;
    const stationIndex = h3.geoToH3(lat, lng, h3Resolution);
    const ring = h3.kRing(stationIndex, radius);
    ring.forEach(h3Index => {
      layer[h3Index] = (layer[h3Index] || 0) + 1;
    });
  });
  return normalizeLayer(layer, true);
}
```

It's often useful to have the "influence" of a given point degrade with distance. H3 makes it relatively simple to assign different values to each concentric ring.

```js
function bufferPointsLinear(geojson, radius) {
  const layer = {};
  geojson.features.forEach(feature => {
    const [lng, lat] = feature.geometry.coordinates;
    const stationIndex = h3.geoToH3(lat, lng, h3Resolution);
    // add surrounding multiple surrounding rings, with less weight in each
    const rings = h3.kRingDistances(stationIndex, radius);
    const step = 1 / (radius + 1);
    rings.forEach((ring, distance) => {
      ring.forEach(h3Index => {
        layer[h3Index] = (layer[h3Index] || 0) + 1 - distance * step;
      });
    });
  });
  return normalizeLayer(layer);
}
```

## Utilities

```js
function normalizeLayer(layer, zeroBaseline = false) {
  const hexagons = Object.keys(layer);
  // Pass one, get max (and min if needed)
  const max = hexagons.reduce((max, hex) => Math.max(max, layer[hex]), -Infinity);
  const min = zeroBaseline
    ? 0
    : hexagons.reduce((min, hex) => Math.min(min, layer[hex]), Infinity);
  // Pass two, normalize
  hexagons.forEach(hex => {
    layer[hex] = (layer[hex] - min) / (max - min);
  });
  return layer;
}
```

```js
function kmToRadius(km) {
  return Math.floor(km / h3.edgeLength(h3Resolution, h3.UNITS.km));
}
```

## Map Rendering

```js
function* map(mapboxgl, mapContainer, config, renderHexes, hexagons) {
  let map = this;
  if (!map) {
    map = new mapboxgl.Map({
      container: mapContainer,
      center: [config.lng, config.lat],
      zoom: config.zoom,
      style: 'mapbox://styles/mapbox/light-v9'
    });
  }

  yield new Promise(resolve => {
    if (map.loaded()) resolve(map);
    else map.on('load', () => resolve(map));
  });

  renderHexes(map, hexagons);
}
```

```js
function renderHexes(map, hexagons) {
  const geojson = geojson2h3.h3SetToFeatureCollection(
    Object.keys(hexagons),
    hex => ({value: hexagons[hex]})
  );

  const sourceId = 'h3-hexes';
  const layerId = `${sourceId}-layer`;
  let source = map.getSource(sourceId);

  if (!source) {
    map.addSource(sourceId, {
      type: 'geojson',
      data: geojson
    });
    map.addLayer({
      id: layerId,
      source: sourceId,
      type: 'fill',
      interactive: false,
      paint: {
        'fill-outline-color': 'rgba(0,0,0,0)',
      }
    });
    source = map.getSource(sourceId);
  }

  source.setData(geojson);

  map.setPaintProperty(layerId, 'fill-color', {
    property: 'value',
    stops: [
      [0, config.colorScale[0]],
      [0.5, config.colorScale[1]],
      [1, config.colorScale[2]]
    ]
  });
  map.setPaintProperty(layerId, 'fill-opacity', config.fillOpacity);
}
```

## Config

```js
const config = {
  lng: -122.2,
  lat: 37.7923539,
  zoom: 10.5,
  fillOpacity: 0.75,
  colorScale: ['#ffffD9', '#50BAC3', '#1A468A']
};
```

## Dependencies

```js
d3Fetch = require('d3-fetch')
mapboxgl = require('mapbox-gl@0.43.0')
h3 = require('https://bundle.run/h3-js@3.1.0')
geojson2h3 = require('https://bundle.run/geojson2h3@1.0.1')
```
~~~

---

## 4. H3 Tutorial: Using Polygon Layers

~~~markdown
# H3 Tutorial: Using Polygon Layers

Many geographic data sets encode their data in polygonal regions. This tutorial demonstrates how to convert polygon layers to sets of H3 hexagons.

## Data

Here we're loading raw data in GeoJSON format. Each element of the data is a polygon representing an Oakland census tract, with a value showing mean travel times (in seconds) from downtown San Francisco. Source: [movement.uber.com](https://movement.uber.com/explore/san_francisco/travel-times)

```js
const sfTravelTimes = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/657a9f3b64fedc718c3882cd4adc645ac0b4cfc5/oakland_travel_times.json'
);
```

Transforming the polygons to hexagons is relatively straightforward using `geojson2h3`. As we know in this case that the polygons don't overlap, each hexagon simply takes the value of the polygon that contains its center.

```js
const hexagons = (() => {
  const layer = {};
  sfTravelTimes.features.forEach(feature => {
    const hexagons = geojson2h3.featureToH3Set(feature, h3Resolution);
    hexagons.forEach(h3Index => {
      layer[h3Index] = feature.properties.travelTime;
    });
  });
  return normalizeLayer(layer);
})();
```

As we've defined our rendering utilities to expect a value between 0 and one, we set up a `normalizeLayer` utility to normalize all the hexagon values to this range.

```js
function normalizeLayer(layer, zeroBaseline = false) {
  const hexagons = Object.keys(layer);
  const max = hexagons.reduce((max, hex) => Math.max(max, layer[hex]), -Infinity);
  const min = zeroBaseline
    ? 0
    : hexagons.reduce((min, hex) => Math.min(min, layer[hex]), Infinity);
  hexagons.forEach(hex => {
    layer[hex] = (layer[hex] - min) / (max - min);
  });
  return layer;
}
```

## Map Rendering

```js
function* map(mapboxgl, mapContainer, config, renderHexes, hexagons) {
  let map = this;
  if (!map) {
    map = new mapboxgl.Map({
      container: mapContainer,
      center: [config.lng, config.lat],
      zoom: config.zoom,
      style: 'mapbox://styles/mapbox/light-v9'
    });
  }

  yield new Promise(resolve => {
    if (map.loaded()) resolve(map);
    else map.on('load', () => resolve(map));
  });

  renderHexes(map, hexagons);
}
```

```js
function renderHexes(map, hexagons) {
  const geojson = geojson2h3.h3SetToFeatureCollection(
    Object.keys(hexagons),
    hex => ({value: hexagons[hex]})
  );

  const sourceId = 'h3-hexes';
  const layerId = `${sourceId}-layer`;
  let source = map.getSource(sourceId);

  if (!source) {
    map.addSource(sourceId, {
      type: 'geojson',
      data: geojson
    });
    map.addLayer({
      id: layerId,
      source: sourceId,
      type: 'fill',
      interactive: false,
      paint: {
        'fill-outline-color': 'rgba(0,0,0,0)',
      }
    });
    source = map.getSource(sourceId);
  }

  source.setData(geojson);

  map.setPaintProperty(layerId, 'fill-color', {
    property: 'value',
    stops: [
      [0, config.colorScale[0]],
      [0.5, config.colorScale[1]],
      [1, config.colorScale[2]]
    ]
  });
  map.setPaintProperty(layerId, 'fill-opacity', config.fillOpacity);
}
```

## Config

```js
const config = {
  lng: -122.2,
  lat: 37.7923539,
  zoom: 10.5,
  fillOpacity: 0.75,
  colorScale: ['#ffffD9', '#50BAC3', '#1A468A']
};
```

## Dependencies

```js
d3Fetch = require('d3-fetch')
mapboxgl = require('mapbox-gl@0.43.0')
h3 = require('https://bundle.run/h3-js@3.1.0')
geojson2h3 = require('https://bundle.run/geojson2h3@1.0.1')
```
~~~

---

## 5. H3 Tutorial: Suitability Analysis

~~~markdown
# H3 Tutorial: Suitability Analysis

Suitability analysis is an approach used in the GIS field to identify optimal geographic locations for some specific use - for example, the best place to locate a new store or transit station. While the process is fairly straightforward - different geographic layers are added together with subjectively defined weights to produce a composite layer - combining arbitrary layers can be challenging. H3 simplifies this problem by providing hexagon grid cells as a common, uniform unit of analysis.

## Data Layers

We'll take each of the raw data layers we're bringing in and convert them to hexagon layers. Each layer will be a map of H3 index to some value normalized between zero and 1.

```js
// Crime layer - Oakland crime reports, last 90 days
const crimeLayer = (() => {
  const layer = {};
  crime90days.forEach(({lat, lng}) => {
    const h3Index = h3.geoToH3(lat, lng, h3Resolution);
    layer[h3Index] = (layer[h3Index] || 0) + 1;
  });
  return normalizeLayer(layer);
})();
```

```js
// Schools layer - Oakland public school locations
const schoolsLayer = (() => {
  const layer = {};
  publicSchools.forEach(({lat, lng}) => {
    const h3Index = h3.geoToH3(lat, lng, h3Resolution);
    // Add school hex
    layer[h3Index] = (layer[h3Index] || 0) + 1;
    // add surrounding kRing, with less weight
    h3.hexRing(h3Index, 1).forEach(neighbor => {
      layer[neighbor] = (layer[neighbor] || 0) + 0.5;
    });
  });
  return normalizeLayer(layer);
})();
```

```js
// BART layer - using buffered point layer with linear falloff
const bartLayer = normalizeLayer(bufferPointsLinear(bartStations, kmToRadius(1)));
```

```js
// Travel time layer - lower travel time is better, so we take the inverse
const travelTimeLayer = (() => {
  const layer = {};
  sfTravelTimes.features.forEach(feature => {
    const hexagons = geojson2h3.featureToH3Set(feature, h3Resolution);
    hexagons.forEach(h3Index => {
      // Lower is better, so take the inverse
      layer[h3Index] = (layer[h3Index] || 0) + 1 / feature.properties.travelTime;
    });
  });
  return normalizeLayer(layer, true);
})();
```

```js
// Cafes layer - Oakland points of interest filtered to food/drink
const cafesLayer = (() => {
  const layer = {};
  pois
    .filter(poi => poi.type === 'Cafes' || poi.type === 'Places to Eat' || poi.type === 'Restaurant')
    .forEach(({lat, lng}) => {
      const h3Index = h3.geoToH3(lat, lng, h3Resolution);
      layer[h3Index] = (layer[h3Index] || 0) + 1;
    });
  return normalizeLayer(layer);
})();
```

## Combining Layers

```js
const mapLayers = [
  {hexagons: schoolsLayer, weight: schoolWeight},
  {hexagons: bartLayer, weight: bartWeight},
  {hexagons: travelTimeLayer, weight: travelWeight},
  // Crime is bad, so we'll subtract it instead of adding
  {hexagons: crimeLayer, weight: -crimeWeight},
  {hexagons: cafesLayer, weight: cafesWeight},
];
```

```js
const combinedLayers = (() => {
  const combined = {};
  mapLayers.forEach(({hexagons, weight}) => {
    Object.keys(hexagons).forEach(hex => {
      combined[hex] = (combined[hex] || 0) + hexagons[hex] * weight;
    });
  });
  return normalizeLayer(combined);
})();
```

## Utilities

```js
function kmToRadius(km) {
  return Math.floor(km / h3.edgeLength(h3Resolution, h3.UNITS.km));
}
```

```js
function bufferPointsLinear(geojson, radius) {
  const layer = {};
  geojson.features.forEach(feature => {
    const [lng, lat] = feature.geometry.coordinates;
    const stationIndex = h3.geoToH3(lat, lng, h3Resolution);
    const rings = h3.kRingDistances(stationIndex, radius);
    const step = 1 / (radius + 1);
    rings.forEach((ring, distance) => {
      ring.forEach(h3Index => {
        layer[h3Index] = (layer[h3Index] || 0) + 1 - distance * step;
      });
    });
  });
  return normalizeLayer(layer);
}
```

```js
function normalizeLayer(layer, baseAtZero = false) {
  const hexagons = Object.keys(layer);
  const max = hexagons.reduce((max, hex) => Math.max(max, layer[hex]), -Infinity);
  const min = baseAtZero
    ? hexagons.reduce((min, hex) => Math.min(min, layer[hex]), Infinity)
    : 0;
  hexagons.forEach(hex => {
    layer[hex] = (layer[hex] - min) / (max - min);
  });
  return layer;
}
```

## Map Rendering

```js
function* map(mapboxgl, mapContainer, config, renderHexes, combinedLayers) {
  let map = this;
  if (!map) {
    map = new mapboxgl.Map({
      container: mapContainer,
      center: [config.lng, config.lat],
      zoom: config.zoom,
      style: 'mapbox://styles/mapbox/light-v9'
    });
  }

  yield new Promise(resolve => {
    if (map.loaded()) resolve(map);
    else map.on('load', () => resolve(map));
  });

  renderHexes(map, combinedLayers);
}
```

```js
function renderHexes(map, hexagons) {
  const geojson = geojson2h3.h3SetToFeatureCollection(
    Object.keys(hexagons),
    hex => ({value: hexagons[hex]})
  );

  const sourceId = 'h3-hexes';
  const layerId = `${sourceId}-layer`;
  let source = map.getSource(sourceId);

  if (!source) {
    map.addSource(sourceId, {
      type: 'geojson',
      data: geojson
    });
    map.addLayer({
      id: layerId,
      source: sourceId,
      type: 'fill',
      interactive: false,
      paint: {
        'fill-outline-color': 'rgba(0,0,0,0)',
      }
    });
    source = map.getSource(sourceId);
  }

  source.setData(geojson);

  map.setPaintProperty(layerId, 'fill-color', {
    property: 'value',
    stops: [
      [0, config.colorScale[0]],
      [0.5, config.colorScale[1]],
      [1, config.colorScale[2]]
    ]
  });
  map.setPaintProperty(layerId, 'fill-opacity', config.fillOpacity);
}
```

## Config

```js
const config = {
  lng: -122.2,
  lat: 37.7923539,
  zoom: 11,
  fillOpacity: 0.9,
  colorScale: ['#ffffD9', '#50BAC3', '#1A468A'],
  areaThreshold: 0.75
};
```

## Raw Data

Oakland crime reports, last 90 days. Source: [data.oaklandnet.com](https://data.oaklandnet.com/Public-Safety/90-Day-Crime-Map/kzer-wcj5)

```js
const crime90days = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/f5ef0fed8972d04a27727ebb50e065265e2d853f/oakland_crime_90days.json'
);
```

Oakland public school locations. Source: [data.oaklandnet.com](https://data.oaklandnet.com/Education/OAKLAND-SCHOOLS/d6pc-iyaw)

```js
const publicSchools = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/babf7357f15c99a1b2a507a33d332a4a87b7df8d/public_schools.json'
);
```

BART station locations. Source: [bart.gov](https://www.bart.gov/schedules/developers/geo)

```js
const bartStations = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/8f1a3e30113472404feebc288e83688a6d5cf33d/bart.json'
);
```

Travel times from Oakland to downtown SF by census tract. Source: [movement.uber.com](https://movement.uber.com/explore/san_francisco/travel-times)

```js
const sfTravelTimes = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/657a9f3b64fedc718c3882cd4adc645ac0b4cfc5/oakland_travel_times.json'
);
```

Oakland points of interest. Source: [uber.com/local](https://www.uber.com/local/us+ca+san_francisco/)

```js
const pois = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/ded89c2acef426fe3ee59b05096ed1baecf02090/oakland-poi.json'
);
```

## Dependencies

```js
mapboxgl = require('mapbox-gl@0.43.0')
h3 = require('https://bundle.run/h3-js@3.1.0')
geojson2h3 = require('https://bundle.run/geojson2h3@1.0.1')
d3Fetch = require('d3-fetch')
```
~~~

---

## 6. H3 Radius Lookup

~~~markdown
# H3 Radius Lookup

You can use H3 for an efficient radius lookup, as long as you're willing to approximate the circle as a k-ring of hexagons (roughly, a large hexagonal shape). This is much faster than a true search by Haversine distance, but less accurate.

## Input

- **H3 Resolution**: 7–10 (default 8). Finer res means more accuracy but more memory usage. Res 8 is roughly a few city blocks in size.
- **Search Radius (Km)**: 0.5–10 (default 4)
- **Grid Radius Calculation Method**: Edge Length or Cell Area

## Logic

### K-Ring Lookup

To use H3 for the radius lookup, you need to index the points of interest by H3 index. If you want this to be efficient, you'd be better off indexing all the points ahead of time.

```js
const lookupMap = bartStations.features.reduce((map, feature) => {
  // Make a map like {[hexagon]: [id, id, ...]}
  const [lon, lat] = feature.geometry.coordinates;
  const h3Index = h3.latLngToCell(lat, lon, res);
  if (!map[h3Index]) map[h3Index] = [];
  map[h3Index].push(feature);
  return map;
}, {});
```

To search, index your search location and get all the H3 indexes within some radius. The functions below demonstrate two methods to go from real-world radius (e.g. meters) to grid radius - one based on edge length, and one based on cell area.

```js
function kRingResults(searchLocation) {
  const lookupIndexes = kRingIndexes(searchLocation);
  // Find all points of interest in the k-ring
  return lookupIndexes.reduce(
    (output, h3Index) => [...output, ...(lookupMap[h3Index] || [])],
    []
  );
}
```

```js
function kRingIndexesEdge(searchLocation, pad = 0) {
  const origin = h3.latLngToCell(searchLocation.lat, searchLocation.lng, res);
  // Transform the radius from km to grid distance
  const radius =
    Math.floor(searchRadiusKm / (h3.getHexagonEdgeLengthAvg(res, h3.UNITS.km) * 2)) + pad;
  return h3.gridDisk(origin, radius);
}
```

```js
function kRingIndexesArea(searchLocation) {
  const origin = h3.latLngToCell(searchLocation.lat, searchLocation.lng, res);
  const originArea = h3.cellArea(origin, h3.UNITS.km2);
  const searchArea = Math.PI * searchRadiusKm * searchRadiusKm;
  let radius = 0;
  let diskArea = originArea;
  while (diskArea < searchArea) {
    radius++;
    const cellCount = 3 * radius * (radius + 1) + 1;
    diskArea = cellCount * originArea;
  }
  return h3.gridDisk(origin, radius);
}
```

### Distance Lookup

One naive alternative is to check every point by Haversine distance, and find those within the radius. This is more accurate, but slower in many cases - it's `O(points)` instead of `O(hexagons)`, and the Haversine calculation is relatively slow compared to H3 indexing and lookup.

```js
function haversineResults(searchLocation) {
  return bartStations.features.filter(
    feature =>
      haversineDistance(
        [searchLocation.lng, searchLocation.lat],
        feature.geometry.coordinates,
        {units: 'kilometers'}
      ) < searchRadiusKm
  );
}
```

## Data

BART station locations in GeoJSON format. Source: [bart.gov](https://www.bart.gov/schedules/developers/geo)

```js
const bartStations = await d3Fetch.json(
  'https://gist.githubusercontent.com/nrabinowitz/d3a5ca3e3e40727595dd137b65058c76/raw/8f1a3e30113472404feebc288e83688a6d5cf33d/bart.json'
);
```

## Dependencies

```js
h3 = require('h3-js@4.1.0')
d3Fetch = require('d3-fetch')
geojson2h3 = require('https://bundle.run/geojson2h3@1.0.1')
mapboxgl = require('mapbox-gl@0.43.0')
makeCircle = require('https://bundle.run/@turf/circle@6.0.1')  // .default
haversineDistance = require('https://bundle.run/@turf/distance@6.0.1')  // .default
```
~~~

---