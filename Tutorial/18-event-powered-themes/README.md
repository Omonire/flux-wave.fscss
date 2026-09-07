# Tutorial 18 — @event-Powered Wave Themes

`@event` returns a value based on conditions — perfect for **theme switching** on waves: dark/light, brand A/B, compact/hero.

## A theme event

```fscss
@event waveTheme(mode) {
  if mode: dark {
    return: #05060a;
  }
  el {
    return: #eef2ff;
  }
}
```

Light/dark of the backdrop, chosen at compile time (or per call site).

## Wiring events into tokens

```fscss
@flux-tokens()

:root {
  --flux-wrap-bg: @event.waveTheme(dark);
  --flux-wrap-radius: 24px;
  --flux-band1-bg: linear-gradient(90deg, transparent, @event.waveGlow(dark) 45%, transparent);
}

@flux-wave-preset(.wave-container)
```

With a second event for glow color:

```fscss
@event waveGlow(mode) {
  if mode: dark { return: #00c2ff; }
  el { return: #4f46e5; }
}
```

## Branching on sizes

```fscss
@event waveHeight(size) {
  if size: hero { return: 45vh; }
  el-if size: divider { return: 180px; }
  el { return: 160px; }
}

:root {
  --flux-wrap-height: @event.waveHeight(hero);
}
```

Add `@event waveBands(n)` when counts must vary per theme (combines with Tutorial 12):

```fscss
@event waveBands(n) {
  if n == 4 { return: flux-i; }
  el { return: duo; }
}
```

## Comparison-driven rating → theme

Events also compare numbers (`>=`, `<`, …):

```fscss
@event waveDensity(layers) {
  if layers >= 6 { return: dense; }
  el { return: sparse; }
}
```

(For generating selectors, pair with `@arr` + `count()` — see Tutorials 12 & 22.)

## Real usage: data-theme attribute switch

FSCSS events resolve at compile points, so for *runtime* theme toggling prefer the custom-property cascade: code both themes, flip a class on an ancestor:

```fscss
:root[data-theme="light"] { --flux-wrap-bg: #eef2ff; }
:root[data-theme="dark"]  { --flux-wrap-bg: #05060a; }
```

`@event` stays the compile-time tool (sub-brand builds, per-media builds); custom properties are the runtime tool. flux-wave works beautifully with both since it reads `var(--flux-*)` at render time.

## Checkpoint

- `@event name(mode) { if ... el-if ... el }` + `@event.name(arg)`.
- Feed event returns directly into `--flux-*` tokens.
- Compile-time themes via events; runtime themes via custom-property cascade on `:root` / containers.

Next — [19 · Modular wave projects](19-modular-wave-projects/README.md)