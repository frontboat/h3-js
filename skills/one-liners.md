Here's every page in the H3 v4 docs with a 1-liner:

### Getting Started

- **`/docs/`** — **Introduction** — H3 is a geospatial indexing system that partitions the world into hexagonal cells, open source under Apache 2.
- **`/docs/installation`** — **Installation** — How to install the H3 C library, or use it via one of its language bindings.
- **`/docs/quickstart`** — **Quick Start** — A quick walkthrough of core H3 functions for getting up and running.

### Highlights

- **`/docs/highlights/indexing`** — **Indexing** — H3 is a hierarchical geospatial index where every cell up to max resolution is uniquely addressable.
- **`/docs/highlights/aggregation`** — **Aggregation** — Using H3's regular grid to bucket and analyze location data like vehicle positions in a city.
- **`/docs/highlights/joining`** — **Joining** — Using H3 as a standard unit of analysis to join disparate datasets together.
- **`/docs/highlights/flowmodel`** — **Flow Modelling** — H3's hexagonal grid is well suited to analyzing movement, with features for modeling flow between cells.
- **`/docs/highlights/ml`** — **Machine Learning** — H3 is well suited to applying ML to geospatial data thanks to its consistent cell properties.

### Comparisons

- **`/docs/comparisons/s2`** — **S2** — Comparing H3 to S2, Google's open source hierarchical discrete global grid using square cells.
- **`/docs/comparisons/geohash`** — **Geohash** — Comparing H3 to Geohash, a hierarchical square grid system encoding locations as character strings.
- **`/docs/comparisons/hexbin`** — **Hexbin** — Comparing H3 to hexbinning, which bins coordinates into configurable hexagonal cells in analytics software.
- **`/docs/comparisons/admin`** — **Admin Boundaries** — Comparing H3 to administrative boundaries (ZIP codes, census blocks) for data aggregation.
- **`/docs/comparisons/placekey`** — **Placekey** — Comparing H3 to Placekey, a POI encoding system that incorporates H3.

### API Reference

- **`/docs/api/indexing`** — **Indexing functions** — Functions for finding the H3 cell index for coordinates, and finding the center/boundary of cells.
- **`/docs/api/inspection`** — **Inspection functions** — Functions providing metadata about an H3 index (resolution, base cell, validity) and 64-bit conversion utilities.
- **`/docs/api/hierarchy`** — **Hierarchy functions** — Functions for moving between resolutions: producing parent (coarser) or child (finer) cells.
- **`/docs/api/traversal`** — **Traversal functions** — Functions for finding cells in the vicinity of an origin cell (k-rings, grid distance, grid paths).
- **`/docs/api/regions`** — **Region functions** — Functions for converting between H3 indexes and polygonal areas.
- **`/docs/api/uniedge`** — **Directed edge functions** — Functions for encoding directed edges between adjacent H3 cells.
- **`/docs/api/vertex`** — **Vertex functions** — Functions for encoding the topological vertexes of H3 cells.
- **`/docs/api/misc`** — **Miscellaneous functions** — Utility functions including grid system descriptions and metrics.

### Concepts and Guides (Library)

- **`/docs/library/terminology`** — **Terminology** — Definitions of key terms used in the H3 system (cell, edge, vertex, resolution, etc.).
- **`/docs/library/errors`** — **Errors** — Reference for H3 error codes and error handling conventions.
- **`/docs/library/index/cell`** — **Cell Index** — Description of the H3 cell index type and its encoding format.
- **`/docs/library/index/directededge`** — **Directed Edge Index** — Description of the H3 directed edge index type and its encoding.
- **`/docs/library/index/vertex`** — **Vertex Index** — Description of the H3 vertex index type and its encoding.
- **`/docs/library/migrating-3.x`** — **Migrating from 3.x** — Guide for migrating code from H3 v3 to v4.
- **`/docs/library/migration-3.x/functions`** — **Function name changes** — Complete mapping of v3 function names to their v4 equivalents.

### Community

- **`/docs/community/bindings`** — **Bindings** — List of language bindings for calling H3 from Python, Java, JavaScript, Go, and more.
- **`/docs/community/libraries`** — **Libraries Using H3** — Third-party libraries that build on H3 via its bindings.
- **`/docs/community/applications`** — **Applications Using H3** — Applications and products that use H3 in production.
- **`/docs/community/tutorials`** — **Learning Resources** — Learning materials, code walkthroughs, and tutorials for H3 and its bindings.

### H3 Internals (Core Library)

- **`/docs/core-library/overview`** — **Overview** — Overview of the H3 C core library architecture.
- **`/docs/core-library/usage`** — **Public API** — The public API is defined in `h3api.h`.
- **`/docs/core-library/restable`** — **Resolution Table** — Table of cell areas, edge lengths, and cell counts at each H3 resolution (0–15).
- **`/docs/core-library/h3Indexing`** — **H3 Indexing** — How H3 cell indexes are encoded internally.
- **`/docs/core-library/coordsystems`** — **Coordinate Systems** — Description of coordinate systems used internally by H3.
- **`/docs/core-library/latLngToCellDesc`** — **latLngToCell** — Internal description of how lat/lng-to-cell indexing works.
- **`/docs/core-library/cellToLatLngDesc`** — **cellToLatLng** — Internal description of how cell-to-lat/lng center point conversion works.
- **`/docs/core-library/cellToBoundaryDesc`** — **cellToBoundary** — Internal description of how cell boundary vertex generation works.
- **`/docs/core-library/filters`** — **Filters** — Command-line filter utilities for working with H3 indexes in pipelines.
- **`/docs/core-library/creating-bindings`** — **Creating Bindings** — Guide for creating new H3 language bindings.
- **`/docs/core-library/custom-alloc`** — **Custom Allocators** — How to use custom memory allocators with the H3 core library.
- **`/docs/core-library/compilation-options`** — **Compilation Options** — CMake compilation flags and build options for the core library.
- **`/docs/core-library/testing`** — **Testing Strategy** — H3's testing approach including unit tests, coverage, and fuzzing.
