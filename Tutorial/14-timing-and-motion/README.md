# Tutorial 14 — Timing & Motion

The wave's personality is almost entirely timing: durations, delays, easing, and the negative-delay trick. Tune these tokens to go from *ambient* to *urgent*.

## The timing tokens

| Token | Default (band 1) | Roll |
|---|---|---|
| `--flux-band-anim` | `flux-flow 4.5s ease-in-out infinite` | the base animation shorthand |
| `--flux-bandN-duration` | 4.2s / 5.1s / 4.8s / 5.6s | each band's loop speed |
| `--flux-bandN-delay` | 0 / −1.1s / −2.3s / −0.7s | each band's head-start |

## Durations — speed as design

The four default durations (4.2, 5.1, 4.8, 5.6s) are all **different on purpose**. If every band ran at the same duration, their phases would lock into a stiff repeat. Divergent durations = continuously shifting overlaps = living liquid.

Try a "fast pulse" variant:

```fscss
:root {
  --flux-band1-duration: 2.4s;
  --flux-band2-duration: 3.1s;
  --flux-band3-duration: 2.8s;
  --flux-band4-duration: 3.6s;
}
```

Try a "slow dream" variant:

```fscss
:root {
  --flux-band1-duration: 9s;
  --flux-band2-duration: 11s;
  --flux-band3-duration: 10s;
  --flux-band4-duration: 12s;
}
```

## Negative delays — instant mid-flow

Set at load, a negative delay starts the animation partway through:

```fscss
--flux-band2-delay: -1.1s;   /* start 1.1s into the loop */
```

Benefits recap:
- No visible "start from frame zero" jump.
- Bands desync *immediately* on page load.
- No extra frames or `opacity:0` hacks needed.

## Easing — character of the motion

Change the whole feel via the eased shorthand:

```fscss
:root {
  --flux-band-anim: flux-flow 4.5s cubic-bezier(0.68, -0.55, 0.27, 1.55) infinite;
}
```

`back-out`-style curves add overshoot bounce; `linear` reads mechanical; `ease-in-out` (default) reads smooth and watery.

## Playing with the loop count

`infinite` is default, but you could make a "one-shot intro wave":

```fscss
:root {
  --flux-band4-anim: flux-flow 4s ease-in-out 1;   /* plays once */
}
```

(Band 4 only — the others keep looping; mix and match for intro effects.)

## Sanity check — the editing rules

- Delay and duration tokens are per-band (`--flux-bandN-...`); the shared `--flux-band-anim` overrides the whole shorthand.
- If you override `--flux-band-anim`, remember it includes the *keyframes name* — keep `flux-flow` unless you've added your own keyframes (Tutorial 22).

## Checkpoint

- Diverge durations or the wave locks into a stiff loop.
- Negative delays = instant desync at load.
- Easing lives in `--flux-band-anim`.

Next — [15 · Multiple waves per page](15-multiple-waves-per-page/README.md)