# Tutorial 25 — Gradient Badge Remix

A complete remix component that borrows every structural idea from flux-wave but ships as its own mini-library: an animated gradient badge. This is flux-wave's architecture applied to a new visual.

## The tokens

```fscss
@define badge-tokens(root:root){`
  :@use(root){
    --badge-size: 96px;
    --badge-bg: #0b0b12;
    --badge-radius: 50%;
    --badge-dur: 3s;

    --badge-c1: #00c2ff;
    --badge-c2: #a855f7;
    --badge-c3: #22c55e;
  }

  @keyframes badge-spin {
    0%   { transform: rotate(0deg) scale(1); }
    50%  { transform: rotate(180deg) scale(1.15); }
    100% { transform: rotate(360deg) scale(1); }
  }
`}
```

## The shell

```fscss
@define badge-base(st: .badge){`
  @use(st){
    position: relative;
    width: var(--badge-size, 96px);
    height: var(--badge-size, 96px);
    border-radius: var(--badge-radius, 50%);
    background: var(--badge-bg, #0b0b12);
    overflow: hidden;
    display: grid;
    place-items: center;
  }
`}
```

## The array-generated sheen layers

Same `count()` + empty-brackets trick as `flux-bands`, but with `::before` pseudo-elements:

```fscss
@define badge-bands(st: .badge){`
@arr band-i[count(3, 1)]
  @use(st)::before:nth-of-type(@arr.band-i[]) {
    $i: @arr.band-i[];
    background: linear-gradient(120deg,
      transparent 0%,
      var(--badge-c@arr.band-i[], var(--badge-c1)) 50%,
      transparent 100%);
    animation: badge-spin var(--badge-dur, 3s) linear infinite;
    animation-delay: num($i! * 0.25)s;
  }
`}
```

Iteration summary (counter = 2):

| Piece | Becomes |
|---|---|
| selector | `.badge::before:nth-of-type(2)` |
| color token | `--badge-c2` |
| delay | `num(2 * 0.25)s` → `0.5s` |

## The preset

```fscss
@define badge-wave(st: .badge){`
  @badge-tokens()
  @badge-base(@use(st))
  @badge-bands(@use(st))
`}
```

## Using it

```fscss
@badge-wave(.status-logo)
```

```html
<div class="status-logo"></div>
```

## Comparing the two architectures

| Concept | flux-wave | badge remix |
|---|---|---|
| tokens + keyframes in one define | `flux-tokens` | `badge-tokens` |
| shell | `flux-container` | `badge-base` |
| shared shape | `flux-band` | (inline in `badge-bands`) |
| generated variants | `flux-bands` (`count(4)`) | `badge-bands` (`count(3)`) |
| preset | `flux-wave-preset` | `badge-wave` |

Same skeleton, different body. That's the reusable pattern this whole course is training you to internalize: **tokens → shell → shape → array-generated layers → preset**.

## Checkpoint

- A remix component reuses the full flux-wave skeleton.
- `$i!` inside loops computes per-layer properties like delay.
- Preset + tokens + arrays = small, data-driven components.

Next — [26 · Shorthand & helpers](26-shorthand-and-helpers/README.md)