# frontboat plugins

A Claude Code plugin marketplace by [frontboat](https://github.com/frontboat).

## Plugins

### h3-js

Equips Claude with the h3-js v4 API so it writes correct H3 code.

- **h3-js** — Resolution guide, core API reference, common patterns, and GeoJSON interop. The intent is to curate a hardened set of docs that address implementation failures most commonly encountered by LLMs.

As always, this skill does not represent uber/h3 nor does it claim the reverse - when in doubt, check the official source at `https://h3geo.org/`.

## Installation

### Add the marketplace

```bash
/plugin marketplace add frontboat/h3-skills
```

### Install a plugin

```bash
/plugin install h3-js@frontboat
```

### Test locally

```bash
claude --plugin-dir ./plugins/h3-js
```

## Credits

All H3 documentation and tutorial content sourced from:

- [h3geo.org](https://h3geo.org/) — H3 official documentation
- [github.com/uber/h3-js](https://github.com/uber/h3-js) — h3-js JavaScript binding
- [@nrabinowitz](https://observablehq.com/@nrabinowitz) — Observable notebooks by Nick Rabinowitz

## License

Apache-2.0 (matching the H3 project)
