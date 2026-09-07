# Tutorial 28 — Publishing Your Wave Library

You've built wave components. Now package them the way flux-wave.fscss is packaged — metadata, working imports, and a doc surface people can actually use.

## The package.json recipe (copy flux-wave's)

```json
{
  "name": "flux-wave",
  "version": "1.0.0",
  "description": "Flowing, layered gradient wave bands built entirely from FSCSS mixins, pure CSS.",
  "extension": "fscss",
  "fscss_version": ">=1.2.0",
  "blocked_methods": [],
  "repository": "https://github.com/you/your-wave.fscss",
  "file": {
    "usage": {
      "directive": "flux-*",
      "helpers": [
        "flux-tokens(root:root)",
        "flux-base(st:.wave-container)",
        "flux-container(st:.wave-container)",
        "flux-band(st:.wave)",
        "flux-bands(st:.wave, counter:flux-i)",
        "flux-wave-preset(st:.wave-container, bandCount:flux-i)"
      ]
    },
    "modules": ["flux-wave.fscss"],
    "remote": "flux-wave.fscss"
  },
  "author": "You",
  "license": "MIT",
  "keywords": ["fscss", "flux", "wave", "gradient", "loader", "css", "no-js"],
  "bugs": "https://github.com/you/your-wave.fscss/issues"
}
```

Key fields for FSCSS:

| Field | Why it matters |
|---|---|
| `"extension": "fscss"` | declares the format |
| `"fscss_version": ">=1.2.0"` | compatibility floor |
| `"file.usage.helpers"` | documents public mixin signatures for tooling/docs |
| `"file.modules"` | the source files loadable as modules |
| `"file.remote"` | the import name for `@import((*) from name)` |

## Wiring the import story

Make *both* import paths work for users:

```fscss
/* by library name (needs module config / package registry) */
@import((*) from your-wave)

/* raw URL (guaranteed to work anywhere) */
@import((*) from "https://raw.githubusercontent.com/you/your-wave/main/your-wave.fscss")
```

For the raw URL, commit the `.fscss` file at the repo root (`your-wave.fscss`) and keep `main` as the default branch.

## README setup (don't over-engineer)

Minimal doc contract:

- What it produces (pure CSS, no JS).
- Install: both import forms.
- Fast usage: `@tokens()` + `@preset(sel)` + the required HTML.
- Customization: the main token overrides table.
- Public mixins table (mirror `file.usage.helpers`).
- Requirements + license.

flux-wave's README in this repo is a good reference.

## Versioning & compatibility

- Follow semver: new mixins = minor; breaking token renames = major.
- Pin `fscss_version` honestly. Test against the runtime and CLI.
- Remember `runtime.min.js` came with v1.2.0 — lower floors need the older exec entry.

## Before you ship — checklist

- [ ] `Package.json` has all fields above.
- [ ] `fscss your.kit.fscss dist.css` compiles cleanly.
- [ ] Browser runtime loads it (Path A from Tutorial 02).
- [ ] Both import paths (`from name` and raw URL) work.
- [ ] README shows a runnable example + token override table.

## Checkpoint

- Package metadata declares format, version floor, module files, and public helpers.
- Ship root-level `.fscss` + default branch `main` for raw-URL imports.
- A runnable README beats a long one.

Next — [29 · The FSCSS ecosystem](29-flux-ecosystem/README.md)