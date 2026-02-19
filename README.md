# h3-js

A Claude Code plugin providing deep expertise in [H3](https://h3geo.org/), Uber's open-source hierarchical hexagonal geospatial indexing system.

## What It Does

Gives Claude comprehensive knowledge of the h3-js v4 API — so it can help you write correct H3 code without hallucinating function names or parameters.

## Skill

- **h3-js** — API cheat sheet with resolution guide, core API reference, and common patterns — plus the complete H3 v4 documentation (API reference, tutorials, Observable notebooks, algorithm internals, comparisons, and migration guides) as a searchable reference file

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