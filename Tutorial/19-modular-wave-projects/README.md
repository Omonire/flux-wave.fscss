# Tutorial 19 — Modular Wave Projects

As your site grows past one page, stop stacking everything in one `.fscss`. Use `@import` to split themes, waves, and page rules into focused files — exactly the architecture flux-wave is designed to slot into.

## A typical wave site layout

```text
styles/
  _tokens.fscss        /* $ vars + @fun stores + :root --flux-* overrides */
  _waves.fscss         /* flux-wave preset calls under your selectors */
  components.fscss     /* navbar, cards, footer */
  main.fscss           /* links everything via @import */
```

## 1 — `_tokens.fscss`

```fscss
@fun(pal){
  deep: #05060a;
  cyan: #00c2ff;
  violet: #a855f7;
}

@flux-tokens()

:root {
  --flux-wrap-bg: @fun.pal.deep.value;
  --flux-wrap-max-width: 100%;
  --flux-band1-bg: linear-gradient(90deg, transparent, @fun.pal.cyan.value 45%, transparent);
  --flux-band2-bg: linear-gradient(90deg, transparent, @fun.pal.violet.value 40%, transparent);
}
```

## 2 — `_waves.fscss`

```fscss
@flux-wave-preset(.wave-hero)
@flux-wave-preset(.wave-footer)
```

## 3 — `main.fscss`

```fscss
@import(exec(_tokens.fscss))
@import(exec(_waves.fscss))

body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background: @fun.pal.deep.value;
  color: #e2e8f0;
}

.container { max-width: 1120px; margin: 0 auto; padding: 0 1rem; }
```

Compile:

```bash
fscss styles/main.fscss dist/main.css
```

One clean CSS file ships; tokens, waves, and layout each stay editable in isolation.

## Selective imports for a vendored flux-wave

If you vendor flux-wave locally instead of importing by library name:

```fscss
@import((
  flux-tokens as tokens,
  flux-wave-preset as preset
) from "vendor/flux-wave.fscss")

@tokens()
@preset(.wave-hero)
```

Aliases keep your file tidy and make swaps painless.

## Flat import by name (default for this repo)

```fscss
@import((*) from flux-wave)
```

Wildcard = bring all six mixins. That's fine and simplest when you actually use them.

## Import hygiene rules

1. Tokens/baseline first — later files in the import list can override.
2. One component-family per file.
3. No circular imports (file A → file B → file A breaks the build).
4. Compile for production; the browser should get bundled CSS, not a chain of links.

## Checkpoint

- Split `_tokens`, `_waves`, components, `main`.
- `@import(exec(file.fscss))` composes them at compile time.
- Selective imports alias flux-wave mixins when vendoring.

Next — [20 · Random & dynamic waves](20-random-and-dynamic-waves/README.md)