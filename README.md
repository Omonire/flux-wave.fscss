# flux-wave.fscss

Flowing, layered gradient wave bands, built entirely from FSCSS mixins. Pure CSS.

## Install

Via remote import:

```fscss
@import((*) from flux-wave)
```

Or from the raw file directly:

```fscss
@import((*) from "https://raw.githubusercontent.com/username/flux-wave.fscss/main/flux-wave.fscss")
```

## Usage

The fastest path is the one-call preset:

```fscss
@import((*) from flux-wave)

@flux-tokens()
@flux-wave-preset(.wave-container)
```

```html
<div class="wave-container">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
</div>
```

`flux-tokens()` writes the shared design tokens (container sizing, per-band gradients, timing) to `:root` and only needs to be called once per page, even with multiple wave containers.

## Multiple wave containers on one page

Every mixin takes a selector parameter, so the preset can run again under a different class:

```fscss
@flux-tokens()
@flux-wave-preset(.wave-hero)
@flux-wave-preset(.wave-footer)
```

## Customizing

Override any token after calling `flux-tokens()` to restyle without touching the mixins:

```fscss
@flux-tokens()

:root {
  --flux-wrap-bg: #0a0a12;
  --flux-band1-bg: linear-gradient(90deg, transparent 0%, #ff2ecb 50%, transparent 100%);
}

@flux-wave-preset(.wave-container)
```

## Changing the number of bands

`flux-bands()` defaults to 4 bands via its own internal `flux-i` array. To use a different count, declare your own array with a real, literal `count()` call and pass its name in:

```fscss
@arr my-bands[count(6,1)]
@flux-bands(.wave, my-bands)
```

Note this only generates the extra selector blocks, any `--flux-bandN-bg` (and other per-band tokens) for bands beyond the default 4 still need to be declared yourself, or they'll fall through to the generic shared defaults.

## Composing manually

If you don't want the full preset, the individual pieces are all public:

```fscss
@flux-tokens()
@flux-base(.my-wave)
@flux-container(.my-wave)
@flux-band(.my-wave .wave)
@flux-bands(.my-wave .wave)
```

## Public mixins

| Mixin | Purpose |
|---|---|
| `flux-tokens(root:root)` | Design tokens (container, per-band gradients, timing), written once |
| `flux-base(st:.wave-container)` | Scoped margin/padding/box-sizing reset |
| `flux-container(st:.wave-container)` | The outer container |
| `flux-band(st:.wave)` | Shared band styling (shape, blend mode, animation) |
| `flux-bands(st:.wave, counter:flux-i)` | The band variants, generated from an index array |
| `flux-wave-preset(st:.wave-container, bandCount:flux-i)` | One-call composite of everything above |

## Requirements

FSCSS `>=1.2.0`.

## License

MIT
