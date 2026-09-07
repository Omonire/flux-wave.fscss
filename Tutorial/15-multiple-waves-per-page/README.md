# Tutorial 15 — Multiple Waves per Page

One `@flux-tokens()` call can feed **many** wave containers. Every mixin takes a selector, so the preset runs again under each class you need.

## The two-in-one-page recipe

```fscss
@flux-tokens()
@flux-wave-preset(.wave-hero)
@flux-wave-preset(.wave-footer)
```

HTML:

```html
<div class="wave-hero">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
</div>

<!-- ...page content... -->

<div class="wave-footer">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
</div>
```

What compiles out: `flux-tokens()` writes ONE set of `:root` tokens and the `flux-flow` keyframes once. Each preset instantiates a full scoped container + bands under its own class. Tokens are inherited by both — zero duplication of token data.

## Why tokens are "once per page"

Custom properties inherit down the tree. `:root` tokens reach every `.wave-hero` and `.wave-footer` child automatically. Calling `@flux-tokens()` twice would re-write identical `:root` blocks (harmless but redundant) — the docs' convention is: call it once, use everywhere.

## Different sizes per wave

Because tokens inherit, you can scope overrides per container... with one caveat: the overrides must be on the container, not `:root`. Custom properties set on the container beat the `:root` value **for that subtree**:

```fscss
@flux-tokens()

:root {
  --flux-wrap-bg: #111;          /* global base */
}

.wave-footer {
  --flux-wrap-height: 90px;      /* footer-specific token */
}

@flux-wave-preset(.wave-hero)
@flux-wave-preset(.wave-footer)
```

`.wave-hero` gets the `:root` height (160px); `.wave-footer` gets 90px. Same pattern works for `--flux-wrap-radius`, per-band colors, durations, anything.

## Brand-scoped overrides via a parent

Wrap once to theme an entire section:

```fscss
.hero-section {
  --flux-band1-bg: linear-gradient(90deg, transparent, #22d3ee 45%, transparent);
  --flux-band2-duration: 6s;
}
```

Any wave *inside* `.hero-section` inherits those tokens; waves outside don't.

## Mixing wave sizes (widget + hero)

```fscss
@flux-tokens()

:root {
  --flux-wrap-max-width: 100%;
}

.wave-widget {
  --flux-wrap-height: 160px;
  --flux-wrap-max-width: 560px;
}

@flux-wave-preset(.hero-splash)
@flux-wave-preset(.wave-widget)
```

## Checkpoint

- `@flux-tokens()` once; `@flux-wave-preset(selector)` per wave.
- Container-scoped tokens override `:root` for that wave only.
- Per-section sidebars plus per-wave sizes = full layout control.

Next — [16 · Variables & waves](16-variables-and-waves/README.md)