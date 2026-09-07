# Tutorial 23 — The Preset Pattern

The preset is the public face of flux-wave: one mixin that composes the whole component, defaults included, so consumers never touch the plumbing.

## Study the original

```fscss
@define flux-wave-preset(st:.wave-container, bandCount: flux-i){`
  @flux-base(@use(st))
  @flux-container(@use(st))
  @flux-band(@use(st) .wave)
  @flux-bands(@use(st) .wave, @use(bandCount))
`}
```

Read it as a recipe:

1. Run the reset under `st`.
2. Style the shell under `st`.
3. Give `st .wave` the shared band shape.
4. Generate `st .wave-N` variants using `bandCount` (defaults to the internal 4-array).

Three of FSCSS's design decisions shine here:

- **Selector threading:** one `st` parameter flows into every inner mixin's selector argument.
- **Defaults everywhere:** `bandCount` defaults to the internal `flux-i`, so `@flux-wave-preset(.wave-container)` just works.
- **Composition over repetition:** the preset has zero styles of its own — it's the sum of its parts.

## Copy the pattern for your component

```fscss
@define glow-badge(st:.blob, count: glow-i){`
  @glow-tokens()
  @glow-shell(@use(st))
  @glow-band(@use(st))
  @glow-bands(@use(st), @use(count))
`}
```

One call from consumers:

```fscss
@glow-badge()                      /* defaults: .blob, 3 bands */
@glow-badge(.mini-glow, duo)       /* another selector, custom count */
```

## Why presets matter for library users

| Without preset | With preset |
|---|---|
| Know all ~6 mixins and call them in order | Call 1 mixin |
| Pass the same `st` five times | Pass it once |
| Risk missing tokens/order | Guaranteed correct assembly |

The preset *encodes* correct usage, so misuse becomes hard.

## Making presets composable too

A preset can itself call other presets — nested composition. In Tutorial 19 you composed page-level presets; nothing stops you nesting component presets:

```fscss
@define glow-page(hero: .wave-hero){`
  @flux-tokens()
  @flux-wave-preset(@use(hero))
  @glow-badge(.mini-glow)
`}
```

## Preset authoring checklist

- [ ] Parameter `st` defaults to the component's canonical class.
- [ ] Any count/generation parameter defaults to an internal array.
- [ ] Token mixin runs first (or is pre-required once per page).
- [ ] Every inner mixin gets `@use(st)` threaded into its selector argument.
- [ ] Bare call `@name()` works with zero arguments.

## Checkpoint

- Preset = composition of reset/shell/shape/variants.
- Thread one `st` parameter through everything.
- Defaults make bare calls work; extra params are optional knobs.

Next — [24 · Wave loaders](24-wave-loaders/README.md)