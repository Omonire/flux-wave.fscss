# Tutorial 11 — Overriding Tokens

flux-wave's superpower: you restyle the entire wave from your own file, **without touching the library**. You override tokens on `:root` after `@flux-tokens()`.

## The override recipe

```fscss
@flux-tokens()

:root {
  --flux-wrap-bg: #0a0a12;
  --flux-band1-bg: linear-gradient(90deg, transparent 0%, #ff2ecb 50%, transparent 100%);
}

@flux-wave-preset(.wave-container)
```

Order matters: tokens first, override second, preset last.

## How it works

`flux-tokens()` writes its defaults to `:root`. Then your `:root` block *re-declares* a few (or many) of them. CSS custom properties resolve to the **last declared value**, so your page's later `:root` wins. All mixins read via `var(--flux-...)` at use time, so they automatically pick up your values.

## Full re-skin example — monochrome "studio" wave

```fscss
@flux-tokens()

:root {
  /* shell */
  --flux-wrap-bg: #111;
  --flux-wrap-height: 200px;
  --flux-wrap-radius: 999px;

  /* shared band sizing */
  --flux-band-height: 72px;
  --flux-band-filter: blur(18px);

  /* per-band gradients + timing */
  --flux-band1-bg: linear-gradient(90deg, transparent, #fafafa 45%, transparent);
  --flux-band1-top: 30%;
  --flux-band2-bg: linear-gradient(90deg, transparent, #9ca3af 40%, transparent);
  --flux-band2-top: 45%;
  --flux-band3-bg: linear-gradient(90deg, transparent, #4b5563 35%, transparent);
  --flux-band3-top: 20%;
}

@flux-wave-preset(.studio-wave)
```

Markup uses the new selector:

```html
<div class="studio-wave">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
</div>
```

## Override rules of thumb

1. **Keep `@flux-tokens()` first** — it's the baseline your overrides build on.
2. **Override as little as possible** — most defaults are already tasteful.
3. **Partial overrides are fine** — do not declare every token unless you want a fully custom theme.
4. **Use the same units** — tokens default to `%` for positions and lengths; mixing `px` in `top` still works but visually is different.

## Common override ideas

| Goal | Override |
|---|---|
| Brand-ify | replace `--flux-bandN-bg` gradients with brand colors |
| Full-bleed hero | `--flux-wrap-max-width: 100%; --flux-wrap-height: 40vh;` |
| Squared look | `--flux-wrap-radius: 8px;` |
| Neon | bright gradients + `--flux-band-blend: screen;` over a near-black bg |
| Pastel | pastel gradients + `--flux-band-opacity` raised |

## Checkpoint

- Overrides belong in *your* `:root`, after `@flux-tokens()`.
- Last-declared-value wins — that's the mechanism.
- You can re-skin the whole wave with ~10 lines, library untouched.

Next — [12 · Changing the band count](12-changing-band-count/README.md)