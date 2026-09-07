# Tutorial 30 — Capstone: Ship a Flux Landing Page

Bring everything together: a themed one-page site with a hero wave, a footer wave, a wave spinner, a gradient badge, and responsive + reduced-motion support. Import flux-wave, override tokens, add your own remix components, compile, and ship.

## Project layout

```text
landing/
  _tokens.fscss
  _waves.fscss
  _remix.fscss      /* badge + spinner from Tutorials 22/25 */
  main.fscss
  index.html
```

## `_tokens.fscss`

```fscss
@fun(pal){
  deep: #05060a;
  cyan: #22d3ee;
  violet: #a855f7;
  rose: #fb7185;
  paper: #0f172a;
}

@flux-tokens()

:root {
  /* page */
  --page-bg: @fun.pal.deep.value;
  --page-ink: #e2e8f0;

  /* waves */
  --flux-wrap-bg: @fun.pal.deep.value;
  --flux-wrap-max-width: 100%;

  --flux-band1-bg: linear-gradient(90deg, transparent, @fun.pal.cyan.value 45%, transparent);
  --flux-band1-duration: 5s;
  --flux-band2-bg: linear-gradient(90deg, transparent, @fun.pal.violet.value 40%, transparent);
  --flux-band2-duration: 6.2s;
  --flux-band3-bg: linear-gradient(90deg, transparent, @fun.pal.rose.value 40%, transparent);
  --flux-band3-duration: 5.4s;
}
```

## `_waves.fscss`

```fscss
.hero-face { --flux-wrap-height: 42vh; }
.footer-face { --flux-wrap-height: 120px; }

@flux-wave-preset(.hero-face)
@flux-wave-preset(.footer-face)
```

## `_remix.fscss`

Paste the `badge-tokens` / `badge-base` / `badge-bands` / `badge-wave` from Tutorial 25 and the `glow-*` trio from Tutorial 22. Then:

```fscss
@badge-wave(.status-halo)
@glow-badge(.boot-spinner)
```

## `main.fscss`

```fscss
@import(exec(_tokens.fscss))
@import(exec(_waves.fscss))
@import(exec(_remix.fscss))

body {
  margin: 0;
  background: var(--page-bg);
  color: var(--page-ink);
  font-family: 'Inter', system-ui, sans-serif;
}

.page { max-width: 1080px; margin: 0 auto; padding: 2rem 1rem; }

/* calm mode */
@media (prefers-reduced-motion: reduce) {
  .hero-face .wave, .footer-face .wave,
  .status-halo, .boot-spinner { animation: none; }
}
```

## `index.html`

```html
<header class="hero-face">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <h1>Flux Landing</h1>
</header>

<main class="page">
  <div class="status-halo"></div>
  <p>Pure-CSS motion, built from FSCSS mixins.</p>
  <div class="boot-spinner">
    <div class="p p-1"></div>
    <div class="p p-2"></div>
    <div class="p p-3"></div>
  </div>
</main>

<footer class="footer-face">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <p>© 2026 — flux training</p>
</footer>

<link type="fscss" href="main.fscss">
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
```

> Wave bands sit behind the text because the containers use flex centering and `overflow: hidden` — the headings layer on top in the flow.

## Compile for production

```bash
fscss landing/main.fscss landing/dist.css
```

Swap the link in `index.html` to `dist.css`, drop the runtime script. You have shipped pure CSS.

## Skills applied across the course

| Tutorial | Technique used here |
|---|---|
| 02 | runtime + CLI compilation |
| 03–07 | tokens, preset, shell, bands |
| 08–10 | flux-flow timing, per-band tuning |
| 11–15 | overrides, sizes, multiple waves per page |
| 16–17 | `$` vars + `@fun` stores powering tokens |
| 19 | modular `@import` files |
| 22–25 | remix components (badge, spinner) |
| 26 | shorthand helpers |
| 27 | responsive + reduced-motion |
| 28 | packaging mindset |

## Final checklist

- [ ] One `@flux-tokens()` for the whole page.
- [ ] Multiple presets (hero, footer) under their own selectors.
- [ ] Your own remix components imported alongside flux-wave.
- [ ] Token-based theming (one `@fun(pal)` drives it all).
- [ ] Responsive + reduced-motion handled.
- [ ] Production compile → single CSS file.

You've completed all 30 flux-wave tutorials. Go wave.