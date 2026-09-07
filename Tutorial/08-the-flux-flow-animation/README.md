# Tutorial 08 — The flux-flow Animation

The visual "flux" comes from a single keyframe animation that every band shares: `flux-flow`. It is emitted as part of `flux-tokens()` inside the backtick block.

## The source

```fscss
@keyframes flux-flow {
  0%   { transform: translateX(-8%) scaleY(0.55) skewX(-4deg); }
  25%  { transform: translateX(-2%) scaleY(1.25) skewX(2deg);  }
  50%  { transform: translateX(6%)  scaleY(0.7)  skewX(-3deg); }
  75%  { transform: translateX(12%) scaleY(1.15) skewX(3deg);  }
  100% { transform: translateX(-8%) scaleY(0.55) skewX(-4deg); }
}
```

Three transforms, five keyframes, one loop.

## Reading each dimension

| Axis | Behavior | Visual effect |
|---|---|---|
| `translateX` | drifts left (−8%) then right (up to +12%) | bands traverse the container window |
| `scaleY` | rubber-bands 0.55 → 1.25 → 0.7 → 1.15 | bands grow/shrink in thickness |
| `skewX` | tilts −4deg ↔ +3deg | bands shear, adding organic lean |

Notice **100% == 0%** exactly. That makes the loop seamless — the last frame is the first frame, so the animation restarts invisibly.

## How bands use it

`flux-band` applies the animation shorthand:

```css
.wave-container .wave {
  animation: var(--flux-band-anim, flux-flow 4.5s ease-in-out infinite);
}
```

Breaking down `flux-flow 4.5s ease-in-out infinite`:

| Piece | Meaning |
|---|---|
| `flux-flow` | the keyframes name |
| `4.5s` | base duration (overridden per band) |
| `ease-in-out` | smooth speed ramp |
| `infinite` | never stops |

Then `flux-bands` adds per-band **delay** and **duration**:

```css
.wave-1 { animation-delay: 0s;    animation-duration: 4.2s; }
.wave-2 { animation-delay: -1.1s; animation-duration: 5.1s; }
.wave-3 { animation-delay: -2.3s; animation-duration: 4.8s; }
.wave-4 { animation-delay: -0.7s; animation-duration: 5.6s; }
```

## The negative-delay trick

Delays like `-2.3s` mean *"start the animation 2.3 seconds in"*. When the page loads, the band is already mid-flow instead of starting everyone at frame zero. That staggered entry is why the wave looks like a living liquid, not a marquee of synchronized blobs.

## First-frame flash, avoided

`animation-delay` on a plain element normally waits (element invisible until animation starts). Negative delays snap the animation to its mid-state *instantly*, so nothing flashes at the wrong size. This is why `flux-wave` never needs an `opacity: 0` pre-hide.

## Overriding motion per site

You can swap easing via a token:

```fscss
:root {
  --flux-band-anim: flux-flow 6s cubic-bezier(0.68, -0.55, 0.27, 1.55) infinite;
}
```

Or slow one band via its duration token:

```fscss
:root {
  --flux-band3-duration: 7s;
}
```

Respect `prefers-reduced-motion` (Tutorial 27) for users who need calmer motion.

## Checkpoint

- `flux-flow` maps translateX / scaleY / skewX over 5 keyframes.
- 100% == 0% guarantees a seamless infinite loop.
- Negative per-band delays de-sync the bands at load.

Next — [09 · Array-generated bands](09-array-generated-bands/README.md)