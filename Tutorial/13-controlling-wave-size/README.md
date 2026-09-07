# Tutorial 13 — Controlling Wave Size

Sizing a wave is a matter of overriding the `--flux-wrap-*` tokens. This tutorial covers the common layouts: standalone widget, full-bleed hero, and responsive scaling.

## The size tokens

| Token | Default | Meaning |
|---|---|---|
| `--flux-wrap-width` | `100%` | how wide the wave stretches |
| `--flux-wrap-max-width` | `560px` | hard cap on width |
| `--flux-wrap-height` | `160px` | wave height |
| `--flux-wrap-radius` | `28px` | corner softness |

## 1. Standalone widget (default feel)

```fscss
:root {
  --flux-wrap-max-width: 560px;
  --flux-wrap-height: 160px;
}
```

Nice centered card — the out-of-box look.

## 2. Full-bleed divider

```fscss
:root {
  --flux-wrap-max-width: 100vw;
  --flux-wrap-width: 100%;
  --flux-wrap-height: 180px;
  --flux-wrap-radius: 0;
}
```

Spans the whole viewport, square corners — a classic page divider between sections.

## 3. Tall hero splash

```fscss
:root {
  --flux-wrap-height: 45vh;
  --flux-wrap-max-width: 100%;
  --flux-wrap-radius: 24px;
}
```

A tall, cinematic band behind hero text (bands will stretch their blur likes; consider boosting `--flux-band-height` so layers still have thickness at `45vh`).

## 4. Responsive scaling with media queries

FSCSS compiles ordinary `@media`, so size can adapt:

```fscss
:root {
  --flux-wrap-height: 160px;
  --flux-wrap-max-width: 560px;
}

@media (min-width: 768px) {
  :root {
    --flux-wrap-height: 220px;
    --flux-wrap-max-width: 720px;
  }
}
```

Note: `@media { :root { ... } }` re-declares tokens (wrapped in their own scope-like block) — the media block wins above the bare `:root` because of ordering.

## 5. Proportional sizing math (FSCSS at play)

Pair token overrides with `num()` math from your own FSCSS:

```fscss
@define hero-wave(st: .hwave){`
  :root {
    --flux-wrap-height: num(100vh * 0.4);
    --flux-wrap-max-width: 100%;
  }
  @flux-wave-preset(@use(st))
`}

@hero-wave(.hwave)
```

`num(100vh * 0.4)` compiles to `40vh` and lands straight into the token. The library itself never demands this, but FSCSS makes it painless.

## Height — diamond width both

Heights aren't limited to `px`: `vh`, `%`, `em`, and `calc` (via `num`) all work because they're just CSS values inside tokens.

## Checkpoint

- Size = `--flux-wrap-*` tokens only.
- Three stock layouts: widget, divider, hero.
- Combine `@media` and `num()` for responsive waves.

Next — [14 · Timing & motion](14-timing-and-motion/README.md)