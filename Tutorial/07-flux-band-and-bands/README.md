# Tutorial 07 — flux-band & flux-bands

These two mixins are the heart of the library: `flux-band` gives *every* layer its shared shape, and `flux-bands` generates the *numbered variants* from an array.

## flux-band — the shared shape

```fscss
@define flux-band(st:.wave){`
  @use(st){
    position: absolute;
    width: var(--flux-band-width, 140%);
    left: var(--flux-band-left, -20%);
    border-radius: var(--flux-band-radius, 50%);
    mix-blend-mode: var(--flux-band-blend, screen);
    animation: var(--flux-band-anim, flux-flow 4.5s ease-in-out infinite);
  }
`}
```

All of it is *shared* — no `wave-N` numbers here. Every band positions itself absolutely, spills out of the frame, rounds into a pill, blends with `screen`, and animates with `flux-flow`.

## flux-bands — the numbered variants

```fscss
@define flux-bands(st:.wave, counter: flux-i){`
@arr flux-i[count(4,1)]
  @use(st).wave-@arr.@use(counter)[]{
    background: var(--flux-band@arr.@use(counter)[]-bg, none);
    top: var(--flux-band@arr.@use(counter)[]-top, 30%);
    height: var(--flux-band@arr.@use(counter)[]-height, var(--flux-band-height, 80px));
    opacity: var(--flux-band@arr.@use(counter)[]-opacity, var(--flux-band-opacity, 0.75));
    filter: var(--flux-band@arr.@use(counter)[]-filter, var(--flux-band-filter, blur(12px)));
    animation-delay: var(--flux-band@arr.@use(counter)[]-delay, 0s);
    animation-duration: var(--flux-band@arr.@use(counter)[]-duration, 4.5s);
  }
`}
```

Decode the interesting syntax:

- `@arr flux-i[count(4,1)]` — declare an index array holding `1, 2, 3, 4`.
- `.wave-@arr.@use(counter)[]` — the *empty brackets* iterate the array, producing selectors `.wave-1`, `.wave-2`, `.wave-3`, `.wave-4`.
- `--flux-band@arr.@use(counter)[]-bg` — inside the block, the same iteration builds token names `--flux-band1-bg`, `--flux-band2-bg`, … per band.

So one loop emits three things you'd otherwise write by hand: the selectors, the token lookups, and their fallbacks.

## What compiles out

```css
.wave-container .wave-1 {
  background: var(--flux-band1-bg, none);
  top: var(--flux-band1-top, 30%);
  height: var(--flux-band1-height, var(--flux-band-height, 80px));
  opacity: var(--flux-band1-opacity, var(--flux-band-opacity, 0.75));
  filter: var(--flux-band1-filter, var(--flux-band-filter, blur(12px)));
  animation-delay: var(--flux-band1-delay, 0s);
  animation-duration: var(--flux-band1-duration, 4.2s);
}
/* ...and identical blocks for wave-2 through wave-4 */
```

## Why this design wins

- **One source of truth:** change `flux-bands`'s structure once and all bands follow.
- **Data-driven:** band count and token names come from the array, not copy-paste.
- **Resilient:** every lookup has a fallback chain (per-band → shared → literal), so a missing token degrades gracefully.

## Gotcha — the counter parameter

`flux-bands` takes `counter: flux-i` — the *name* of an array to iterate. The default `flux-i` is the internal 4-item array. If you pass a different array (Tutorial 12) you change how many selectors are generated, but the mixin still reads `--flux-bandN-*` tokens for each N. The two must stay consistent.

## Checkpoint

- `flux-band` = shared shape; `flux-bands` = per-number variants.
- Empty brackets + `count(4,1)` generate 4 selectors and 4 token lookups.
- Band count and token suffixes must stay in sync.

Next — [08 · The flux-flow animation](08-the-flux-flow-animation/README.md)