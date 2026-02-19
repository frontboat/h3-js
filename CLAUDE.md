# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

H3 is Uber's hexagonal hierarchical geospatial indexing system. It's a pure C library (C99) that maps latitude/longitude coordinates to hexagonal grid cells at 16 resolutions (0-15). The core type `H3Index` is a `uint64_t` with bit-packed fields encoding cell identity.

## Build Commands

```bash
# Build
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make

# Run all tests
cd build && ctest

# Run a single test
cd build && ctest -R testGridDisk

# Fast test subset (excludes expensive tests)
cd build && make test-fast

# Format code (requires clang-format-14)
cd build && make format

# Build with coverage
cmake -DCMAKE_BUILD_TYPE=Debug -DENABLE_COVERAGE=ON ..
make && make coverage
```

Useful CMake options: `-DWARNINGS_AS_ERRORS=ON`, `-DENABLE_LINTING=ON` (clang-tidy), `-DBUILD_BENCHMARKS=ON`.

## Architecture

### Core Library (`src/h3lib/`)

- `include/` — Headers (public API + internal). `h3api.h.in` is the public API template (CMake generates `h3api.h`).
- `lib/` — Implementation files, 1:1 correspondence with headers.

Key modules:
- **h3Index** — H3Index bit manipulation (H3_GET_*/H3_SET_* macros for mode, resolution, base cell, digits)
- **latLng** — Geodetic coordinate conversions
- **faceijk/coordijk** — Icosahedral face and IJK coordinate systems (the math core)
- **baseCells** — 122 base cells on the icosahedron, lookup tables
- **algos** — Grid traversal algorithms (gridDisk, gridRing, gridPath)
- **polyfill/polygon** — Polygon fill operations
- **cellsToMultiPoly/linkedGeo** — Converting cell sets back to polygon geometry
- **directedEdge** — Edge functions between neighboring cells
- **vertex** — Cell vertex operations

### Applications (`src/apps/`)

- `testapps/` — Test programs (~50 files), each is a standalone executable with `SUITE`/`TEST` macros
- `fuzzers/` — AFL++/libFuzzer harnesses (~30 files)
- `benchmarks/` — Performance benchmarks
- `filters/` — CLI utilities (cellToLatLng, latLngToCell, etc.)
- `miscapps/` — Code generation tools (generateBaseCellNeighbors, generatePentagonDirectionFaces, etc.)

### Test configuration

Tests are defined in `CMakeTests.cmake` using `add_h3_test()` and `add_h3_memory_test()` macros. Each test is a standalone C program using the harness in `src/apps/applib/include/test.h`.

## Code Conventions

**API pattern:** All public functions return `H3Error` (uint32_t enum). Output is written via pointer parameters. Public symbols use the `H3_EXPORT(functionName)` macro.

```c
H3Error H3_EXPORT(latLngToCell)(const LatLng *g, int res, H3Index *out);
```

**Internal functions** are prefixed with underscore (`_functionName`).

**No dynamic allocation** in the core library — all buffers are caller-provided or stack-allocated. Custom allocators supported via `H3_ALLOC_PREFIX`.

**Formatting:** Google style base, 4-space indent, right-aligned pointers (`int *p`). Enforced by clang-format-14 — other versions may produce different output and fail CI.

**Test macros:** `SUITE(name)` defines main(), `TEST(name)` sets test name, `t_assert(cond, msg)` checks conditions, `t_assertSuccess(expr)` checks for `E_SUCCESS`.

## Quality Requirements

- 100% code coverage required for the core H3 library
- Must compile with GCC, Clang, and MSVC
- New public API functions need fuzzer tests
- Changes need a CHANGELOG.md entry under the Unreleased section
