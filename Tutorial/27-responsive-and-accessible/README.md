# Tutorial 27 — Responsive & Accessible Waves

Waves should adapt to viewports and respect users who prefer reduced motion. FSCSS compiles ordinary `@media` rules, so responsiveness is straightforward.

## Responsive shells

Sweep token overrides in media blocks:

```fscss
@flux-tokens()

:root {
  --flux-wrap-height: 140px;
  --flux-wrap-max-width: 560px;
}

@media (min-width: 768px) {
  :root {
    --flux-wrap-height: 200px;
    --flux-wrap-max-width: 720px;
  }
}

@media (min-width: 1200px) {
  :root {
    --flux-wrap-height: 300px;
    --flux-wrap-max-width: 100%;
  }
}

@flux-wave-preset(.wave-container)
```

The media-scoped `:root` re-declarations override the base by ordering — later wins.

## Scope overrides per component, not :root

Prefer component-scoped tokens so multiple waves on the page scale independently:

```fscss
@media (max-width: 600px) {
  .wave-hero { --flux-wrap-height: 120px; }
  .wave-footer { --flux-wrap-height: 64px; }
}
```

Waves outside these selectors keep their normal sizes.

## Responsive band density

Fewer bands on small screens = lighter rendering:

```fscss
.hero-wave { --flux-band4-opacity: 0.15; }
@media (max-width: 600px) {
  .hero-wave { --flux-band3-opacity: 0.2; }
}
```

(Or use fewer `.wave` divs in mobile markup — CSS-only tuning keeps it simpler.)

## prefers-reduced-motion

flux-wave is *pure animation*. Honor system settings by freezing it:

```fscss
@media (prefers-reduced-motion: reduce) {
  .wave-container .wave {
    animation: none;
    opacity: 0.6;   /* keep a calm static glow */
  }
}
```

Better still — ship from your theme file once:

```fscss
@define calm-waves(st: .wave-container){`
  @media (prefers-reduced-motion: reduce) {
    @use(st) .wave { animation: none; }
  }
`}

@calm-waves()
```

Build on every wave container by default.

## Accessibility checklist

- [ ] Reduced-motion: bands become static (still readable).
- [ ] Contrast: `--flux-wrap-bg` + band opacities keep any overlaid text legible.
- [ ] Don't rely on waves for meaning — always pair with text/aria labels.
- [ ] Waves behind content stay `overflow: hidden` (no scrollbar escape).

## Checkpoint

- `@media` + token overrides scale waves at breakpoints.
- Component-scoped overrides keep multiple waves independent.
- `prefers-reduced-motion` freezes bands to static glow.

Next — [28 · Publishing your wave](28-publishing-your-wave/README.md)