# Tutorial 29 — The FSCSS Ecosystem

flux-wave doesn't exist in a vacuum. It rides the FSCSS module ecosystem — libraries published as `.fscss` packages that compose like building blocks.

## What's out there

- **FSCSS itself** — docs, examples, API references: https://fscss.devtem.org
- **Official module library** — the `fscss-modules` collection (progress, navigation, UI kits): https://github.com/fscss-ttr/fscss-modules
- **This repo** — flux-wave.fscss, a gradient-wave component.
- More modules and ideas appear on dev.to / the fscss.dev community as `fscss-*` packages.

## Importing from the registry vs raw URL

The ecosystem has two import idioms:

```fscss
/* Registry-style by name (after installing/configuring modules) */
@import((
  circle-progress as clp,
  progress-range as pr
) from circle-progress)

/* Raw remote file (works anywhere, no registry) */
@import((*) from "https://raw.githubusercontent.com/fscss-ttr/.../file.fscss")
```

flux-wave supports both — the README shows `from flux-wave` and the raw URL.

## Composing multiple modules on one page

Waves + progress + nav can coexist because every mixin takes a selector:

```fscss
@import((*) from flux-wave)
@import((
  progress-circle as pCircle
) from circle-progress)

@flux-tokens()
@flux-wave-preset(.wave-hero)

.pc {
  @pCircle(72)
}
```

Separate selectors, zero collisions.

## Selective imports to keep namespaces clean

If two modules export a same-named mixin, alias on import (Tutorial 18/19):

```fscss
@import((
  flux-wave-preset as fw
) from flux-wave)

@fw(.wave-hero)
```

## Conventions to follow as a contributor

When publishing your own wave module to the ecosystem:

- Prefix public mixins with the module name (`flux-*`, `badge-*`, …) so imports don't collide.
- Ship `package.json` metadata (`extension`, `fscss_version`, `file.modules`).
- Provide a `-preset` one-call composite (Tutorial 23).
- Use data-driven arrays (Tutorial 09/22) instead of copy-paste variants.
- Document helpers in `file.usage.helpers`.

## Where flux-wave sits

flux-wave is a **no-JS, pure-CSS animation component** — a gradient/loading/decoration module. It shows how far you can push FSCSS arrays + tokens + keyframes without any scripting, and it's a template for publishing your own.

## Discovering more

- GitHub: `fscss-ttr` org + community repos tagged `fscss`.
- dev.to: `fscss-ttr` publication posts tutorials & showcases.
- The docs community page lists each channel: https://fscss.devtem.org/docs

## Checkpoint

- The ecosystem = FSCSS core + `fscss-*` modules + community packages.
- Compose modules on one page via selector-parameterized presets.
- Publish convention: prefix mixins, metadata, preset, docs.

Next — [30 · Capstone: flux landing](30-capstone-flux-landing/README.md)