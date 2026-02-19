---
name: h3-js
description: Use when working with H3 hexagonal geospatial indexing, h3-js, hex cells, spatial analysis, resolution selection, or any Uber H3 API usage in JavaScript. Includes full H3 v4 documentation reference.
---

# H3 Geospatial Indexing with h3-js

H3 is Uber's open-source hierarchical hexagonal geospatial indexing system. The h3-js library (v4) provides the JavaScript binding, transpiled from C via emscripten.

Install: `npm install h3-js`

Import: `import * as h3 from 'h3-js'` (ESM) or `const h3 = require('h3-js')` (CJS)

## Resolution Guide

H3 has 16 resolutions (0-15). Each finer resolution has ~7x more cells:

| Res | Avg Hex Area | Avg Edge Length | Use Case |
|-----|-------------|-----------------|----------|
| 0 | 4,357,449 km2 | 1,281 km | Continental |
| 3 | 12,393 km2 | 69 km | Country/state |
| 5 | 253 km2 | 9.9 km | Metro area |
| 7 | 5.16 km2 | 1.4 km | Neighborhood |
| 9 | 0.105 km2 | 201 m | City block |
| 11 | 0.00215 km2 | 28.7 m | Building |
| 13 | 43.9 m2 | 4.1 m | Room |
| 15 | 0.9 m2 | 0.58 m | Sub-meter |

## Core API (h3-js v4)

### Indexing

```js
// Lat/lng to H3 cell index
h3.latLngToCell(lat, lng, resolution)
// → "85283473fffffff"

// Cell center point
h3.cellToLatLng(cell)
// → [lat, lng]

// Cell boundary vertices (pass true for GeoJSON lng/lat order + closed loop)
h3.cellToBoundary(cell, formatAsGeoJson?)
// → [[lat, lng], ...]
```

### Inspection

```js
h3.getResolution(cell)        // → number (0-15)
h3.isValidCell(cell)           // → boolean
h3.isPentagon(cell)            // → boolean
h3.isResClassIII(cell)         // → boolean
h3.getBaseCellNumber(cell)     // → number (0-121)
h3.getIcosahedronFaces(cell)   // → [faceNum, ...]
```

### Hierarchy

```js
// Parent at coarser resolution
h3.cellToParent(cell, parentRes)

// Children at finer resolution
h3.cellToChildren(cell, childRes)

// Center child at finer resolution
h3.cellToCenterChild(cell, childRes)

// Compact/uncompact cell sets (all cells must share same resolution)
h3.compactCells(cells)            // → mixed-resolution compact set
h3.uncompactCells(cells, res)     // → uniform-resolution expanded set

// Child position encoding
h3.cellToChildPos(child, parentRes)          // → position index
h3.childPosToCell(childPos, parent, childRes) // → cell
```

### Traversal

```js
// Filled disk of cells within k steps
h3.gridDisk(origin, k)
// → [cell, ...]

// Disk grouped by distance (array of arrays, index = distance)
h3.gridDiskDistances(origin, k)
// → [[ring0], [ring1], ...]

// Hollow ring at exactly k steps
h3.gridRing(origin, k)

// Grid distance between two cells (same resolution)
h3.gridDistance(a, b)
// → number

// Path of cells between two cells (inclusive)
h3.gridPathCells(start, end)

// Local IJ coordinates (experimental)
h3.cellToLocalIj(origin, cell)    // → {i, j}
h3.localIjToCell(origin, {i, j})  // → cell
```

### Regions (Polygon <-> Cells)

```js
// Polygon to cells (polygon = array of [lat, lng] rings)
// First ring is outer boundary, subsequent are holes
h3.polygonToCells(polygon, res, isGeoJson?)
// → [cell, ...]

// Cells to multipolygon outline
h3.cellsToMultiPolygon(cells, formatAsGeoJson?)
// → [[[[lat, lng], ...], ...], ...]

// Experimental: containment modes (center, full, overlapping, overlapping_bbox)
h3.polygonToCellsExperimental(polygon, res, flags, isGeoJson?)
```

### Directed Edges

```js
h3.areNeighborCells(a, b)                // → boolean
h3.cellsToDirectedEdge(origin, dest)     // → edge index
h3.getDirectedEdgeOrigin(edge)           // → cell
h3.getDirectedEdgeDestination(edge)      // → cell
h3.directedEdgeToCells(edge)             // → [origin, dest]
h3.originToDirectedEdges(cell)           // → [edge, ...]
h3.directedEdgeToBoundary(edge)          // → [[lat, lng], ...]
```

### Vertexes

```js
h3.cellToVertex(cell, vertexNum)   // → vertex index (0-5 hex, 0-4 pentagon)
h3.cellToVertexes(cell)            // → [vertex, ...]
h3.vertexToLatLng(vertex)          // → [lat, lng]
h3.isValidVertex(vertex)           // → boolean
```

### Metrics & Utilities

```js
h3.getHexagonAreaAvg(res, h3.UNITS.km2)         // avg hex area
h3.getHexagonEdgeLengthAvg(res, h3.UNITS.km)    // avg edge length
h3.cellArea(cell, h3.UNITS.km2)                  // exact cell area
h3.edgeLength(edge, h3.UNITS.km)                 // exact edge length
h3.getNumCells(res)                              // total cells at resolution
h3.greatCircleDistance(point1, point2, h3.UNITS.km)  // haversine distance
h3.getRes0Cells()                                // all 122 base cells
h3.getPentagons(res)                             // 12 pentagons at resolution
h3.degsToRads(degrees)
h3.radsToDegs(radians)
```

## Common Patterns

### Radius Search via k-Ring

```js
const origin = h3.latLngToCell(lat, lng, 8);
const radiusKm = 4;
const edgeKm = h3.getHexagonEdgeLengthAvg(8, h3.UNITS.km);
const k = Math.floor(radiusKm / (edgeKm * 2));
const nearby = h3.gridDisk(origin, k);
```

### Point Layer to Heatmap

```js
function pointsToLayer(points, res) {
  const layer = {};
  points.forEach(({lat, lng}) => {
    const cell = h3.latLngToCell(lat, lng, res);
    layer[cell] = (layer[cell] || 0) + 1;
  });
  return layer;
}
```

### Buffered Points with Distance Falloff

```js
function bufferPointsLinear(points, res, kRadius) {
  const layer = {};
  points.forEach(({lat, lng}) => {
    const origin = h3.latLngToCell(lat, lng, res);
    const rings = h3.gridDiskDistances(origin, kRadius);
    const step = 1 / (kRadius + 1);
    rings.forEach((ring, dist) => {
      ring.forEach(cell => {
        layer[cell] = (layer[cell] || 0) + 1 - dist * step;
      });
    });
  });
  return layer;
}
```

### Polygon Layer to Cells

```js
function polygonToHexLayer(geojsonFeatures, res) {
  const layer = {};
  geojsonFeatures.forEach(feature => {
    const coords = feature.geometry.coordinates;
    const cells = h3.polygonToCells(coords[0], res, true);
    cells.forEach(cell => {
      layer[cell] = feature.properties.value;
    });
  });
  return layer;
}
```

## Key Concepts

- **12 pentagons** exist at every resolution (icosahedron vertices). Most functions handle them transparently, but `gridRing` may throw near them (use `gridDisk` as a safe alternative).
- **Hierarchical containment is approximate**: child cell centers may fall outside parent cell geometry. Use `cellToParent` for logical hierarchy.
- **Class III vs Class II**: odd resolutions are Class III (rotated 19.1 degrees). Adjacent resolutions alternate aperture 7.
- **v3 to v4 migration**: function names changed (e.g., `geoToH3` → `latLngToCell`, `kRing` → `gridDisk`). See `# === library/migration-3.x/functions ===` in reference.md for the complete mapping.

## Full Documentation

For deep dives — full API reference, tutorials, algorithm internals, comparisons, migration guides, and 40+ Observable notebooks — see [reference.md](reference.md). Sections are searchable by path (e.g., `# === api/traversal ===`).
