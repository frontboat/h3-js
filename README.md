# h3-js

A Claude Code plugin for [H3](https://h3geo.org/), Uber's hexagonal geospatial indexing system.

## What It Does

Equips Claude with the h3-js v4 API so it writes correct H3 code instead of hallucinating function names.

## Skill

- **h3-js** — Resolution guide, core API reference, common patterns, and GeoJSON interop. Backed by full H3 v4 documentation (algorithm internals, tutorials, Observable notebooks, migration guides) as a searchable reference file

## Installation

### Claude Code

```bash
claude plugins add h3-js
```

Or test locally:

```bash
claude --plugin-dir /path/to/h3-skills
```

## Credits

All H3 documentation and tutorial content sourced from:
- [h3geo.org](https://h3geo.org/) — H3 official documentation
- [github.com/uber/h3-js](https://github.com/uber/h3-js) — h3-js JavaScript binding
- [@nrabinowitz](https://observablehq.com/@nrabinowitz) — Observable notebooks by Nick Rabinowitz

## License

Apache-2.0 (matching the H3 project)