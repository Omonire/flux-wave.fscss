# Tutorial 24 — Wave Loaders

flux-wave's technique is a natural fit for **loading states**: blurred gradient layers in motion. Build three loaders — a spinner, a bar, and a boot screen — all from the wave bag of tricks.

## Loader 1 — a flux-style spinner

Token preset:

```fscss
@flux-tokens()

:root {
  --flux-wrap-width: 180px;
  --flux-wrap-height: 180px;
  --flux-wrap-radius: 50%;
  --flux-wrap-bg: #101018;
  --flux-band-radius: 50%;
  --flux-band-filter: blur(10px);
  --flux-band-opacity: 0.7;
}

@flux-wave-preset(.loader-spin)
```

```html
<div class="loader-spin">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
</div>
```

Two bands inside a circular dark shell already read as "waiting." Add a center note (HTML ignores overflow content with flex centering):

```html
<div class="loader-spin">…waves…<span class="note">loading</span></div>
```

## Loader 2 — indeterminate progress bar

Reuse wave bands as a progress blob:

```fscss
@flux-tokens()

:root {
  --flux-wrap-width: 320px;
  --flux-wrap-height: 12px;
  --flux-wrap-radius: 999px;
  --flux-wrap-bg: #1e293b;
  --flux-band-height: 16px;
  --flux-band1-opacity: 1;
  --flux-band1-bg: linear-gradient(90deg, transparent, #22d3ee 50%, transparent);
}

@flux-wave-preset(.loader-bar)
```

The `flux-flow` translateX drift becomes the moving highlight. `overflow: hidden` keeps it inside the pill.

## Loader 3 — boot screen panel

Combine a tall wave with a small fixed spinner:

```fscss
@flux-tokens()

:root {
  --flux-wrap-height: 50vh;
  --flux-wrap-max-width: 100%;
  --flux-wrap-radius: 0;
}

@flux-wave-preset(.boot-wave)

@glow-badge(.boot-spinner)   /* from Tutorial 22 */
```

```html
<div class="boot-wave">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
</div>
<div class="boot-spinner">
  <div class="p p-1"></div>
  <div class="p p-2"></div>
  <div class="p p-3"></div>
</div>
```

## Adding motion preference (forward pointer)

Accessibility for loaders: calm the animation for reduced-motion users (full coverage in Tutorial 27):

```fscss
@media (prefers-reduced-motion: reduce) {
  .loader-spin .wave, .boot-wave .wave {
    animation: none;
  }
}
```

## The loading-state checklist

- [ ] Size tokens tuned (`height`, `radius`) to the loading shape.
- [ ] Band count minimal (1–3) so states feel light.
- [ ] `overflow: hidden` on the shell.
- [ ] Reduced-motion fallback applied.

## Checkpoint

- The wave recipe re-skins into spinner, bar, and boot screens.
- Centering + `overflow hidden` lets you drop UI text inside shells.
- Always degrade motion for `prefers-reduced-motion`.

Next — [25 · Gradient badge remix](25-gradient-badge-remix/README.md)