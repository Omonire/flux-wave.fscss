# Tutorial 17 — @fun Stores for Waves

`@fun(name){ key: value; }` is FSCSS's **token store**: a named group of key/value pairs read with dot access. It's the ideal place to keep a wave's palette and spacing before they become `--flux-*` overrides.

## A wave palette store

```fscss
@fun(wave-pal){
  deep: #05060a;
  cyan: #00c2ff;
  violet: #a855f7;
  rose: #ff3d9a;
  sky: #64d2ff;
}

@fun(wave-motion){
  slow: 7s;
  mid: 5.5s;
  fast: 4s;
}
```

## Feeding stores into tokens

Read a single value with `.property.value` and emit into `:root`:

```fscss
@flux-tokens()

:root {
  --flux-wrap-bg: @fun.wave-pal.deep.value;

  --flux-band1-bg: linear-gradient(90deg, transparent, @fun.wave-pal.cyan.value 45%, transparent);
  --flux-band2-bg: linear-gradient(90deg, transparent, @fun.wave-pal.violet.value 40%, transparent);
  --flux-band3-bg: linear-gradient(90deg, transparent, @fun.wave-pal.rose.value 40%, transparent);

  --flux-band1-duration: @fun.wave-motion.mid.value;
  --flux-band2-duration: @fun.wave-motion.slow.value;
  --flux-band3-duration: @fun.wave-motion.fast.value;
}

@flux-wave-preset(.wave-container)
```

Change the store, recompile, wave re-themes. One store = whole page palette.

## Access-level recap (used above)

| Expression | Returns |
|---|---|
| `@fun.wave-pal.deep` | the pair `deep: #05060a;` |
| `@fun.wave-pal.deep.value` | just the value `#05060a` |

Inside a `linear-gradient(...)` or a token value you almost always want `.value`.

## Fallback merging with flux defaults

Your overrides only need to supply what you override. Tokens that fall through remain the library's shared defaults — combine store-driven colors with untouched motion tokens:

```fscss
:root {
  --flux-wrap-bg: @fun.wave-pal.deep.value;
  --flux-band1-bg: linear-gradient(90deg, transparent, @fun.wave-pal.cyan.value 45%, transparent);
}
/* delays, durations, blur, opacity all stay default */
```

## Building a scoped "mini-preset" with @fun + @define

```fscss
@fun(skin){
  A: #22d3ee;
  B: #a855f7;
  C: #fb7185;
}

@define skinned-wave(st, durA, durB){`
  :root {
    --flux-band1-bg: linear-gradient(90deg, transparent, @fun.skin.A.value 45%, transparent);
    --flux-band2-bg: linear-gradient(90deg, transparent, @fun.skin.B.value 40%, transparent);
    --flux-band1-duration: @use(durA);
    --flux-band2-duration: @use(durB);
  }
  @flux-wave-preset(@use(st))
`}
```

Call once, reuse for every matching page section.

## When to use @fun vs $ variables

| Tool | Use for |
|---|---|
| `$brand: #ff2ecb;` | single ad-hoc values in a file |
| `@fun(pal){...}` | named, reusable **collections** that other files import |

For flux-wave theming at scale, `@fun` stores read far better than 20 loose `$`.

## Checkpoint

- Stores: `@fun(name){ key: value; }`.
- Read values: `@fun.name.key.value` → plug into tokens.
- Combine with `@define` to build repeatable themed waves.

Next — [18 · @event-powered themes](18-event-powered-themes/README.md)