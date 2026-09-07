# Tutorial 10 — Per-Band Styling

Looks aren't magic — they're seven per-band tokens, tuned differently for each of the four layers. This tutorial reads the actual defaults so you can start *designing* bands instead of just watching them.

## The seven dials per band

Every band N reads seven tokens (all with fallback chains):

| Token | What it controls |
|---|---|
| `--flux-bandN-bg` | the gradient wash |
| `--flux-bandN-top` | vertical placement inside the 160px container |
| `--flux-bandN-height` | band thickness |
| `--flux-bandN-opacity` | transparency |
| `--flux-bandN-filter` | blur strength |
| `--flux-bandN-delay` | animation head-start (negative ok) |
| `--flux-bandN-duration` | loop speed |

## The shipping defaults

### Band 1 — the headline layer

```fscss
--flux-band1-bg: linear-gradient(90deg, transparent 0%, #00c2ff 20%, #30d158 40%, #af52de 60%, #ff375f 80%, transparent 100%);
--flux-band1-top: 35%;
--flux-band1-height: 80px;    /* via shared default --flux-band-height */
--flux-band1-opacity: 0.75;   /* via shared default */
--flux-band1-filter: blur(12px);
--flux-band1-delay: 0s;
--flux-band1-duration: 4.2s;
```

### Band 2 — desynced, taller but softer

```fscss
--flux-band2-bg: linear-gradient(90deg, transparent 0%, #0a84ff 25%, #c42bff 50%, #ff2d55 75%, transparent 100%);
--flux-band2-top: 28%;
--flux-band2-height: 70px;
--flux-band2-opacity: 0.65;
--flux-band2-filter: blur(12px);   /* shared */
--flux-band2-delay: -1.1s;
--flux-band2-duration: 5.1s;
```

### Band 3 — the slow, dim middle

```fscss
--flux-band3-bg: linear-gradient(90deg, transparent 0%, #34c759 30%, #5e5ce6 55%, #ff375f 80%, transparent 100%);
--flux-band3-top: 42%;
--flux-band3-height: 65px;
--flux-band3-opacity: 0.55;
--flux-band3-filter: blur(12px);
--flux-band3-delay: -2.3s;
--flux-band3-duration: 4.8s;
```

### Band 4 — the fuzziest, most transparent

```fscss
--flux-band4-bg: linear-gradient(90deg, transparent 0%, #64d2ff 20%, #bf5af2 45%, #ff9f0a 70%, transparent 100%);
--flux-band4-top: 38%;
--flux-band4-height: 55px;
--flux-band4-opacity: 0.45;
--flux-band4-filter: blur(16px);   /* extra soft */
--flux-band4-delay: -0.7s;
--flux-band4-duration: 5.6s;
```

## Why these exact numbers produce "flux"

1. **Diverse durations (4.2s–5.6s):** each band loops at its own speed, so their phases are always different.
2. **Diverse delays & negative values:** at load, each band is already mid-flight — nobody starts synchronized.
3. **Diminishing opacities (0.75 → 0.45) + rising blur:** deeper layers are softer, creating depth.
4. **Staggered tops (28%–42%) and heights (55–80px):** the bands stack like a folded ribbon, not a stack of copies.

## Designing your own band set

A minimal recipe for a new wave look — pick a hue family, vary lightness per band, desync everything:

```fscss
:root {
  --flux-band1-bg: linear-gradient(90deg, transparent, #f43f5e 30%, transparent);
  --flux-band1-top: 30%;
  --flux-band1-delay: 0s;        --flux-band1-duration: 5s;

  --flux-band2-bg: linear-gradient(90deg, transparent, #8b5cf6 40%, transparent);
  --flux-band2-top: 40%;
  --flux-band2-delay: -1.6s;     --flux-band2-duration: 6s;

  --flux-band3-bg: linear-gradient(90deg, transparent, #06b6d4 35%, transparent);
  --flux-band3-top: 25%;
  --flux-band3-delay: -3s;       --flux-band3-duration: 7s;
}
```

Notice: `height`, `opacity`, `filter` can be *left alone* and fall through to shared defaults — you only override what your design needs.

## Checkpoint

- Name all seven per-band dials.
- Explain why opacities dim and durations differ across bands.
- You can craft a custom 3–4 band color/delay set via `:root`.

Next — [11 · Overriding tokens](11-overriding-tokens/README.md)