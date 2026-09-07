# Tutorial 08 — Composition with `@define`

Once you have several defines, you can apply **multiple defines inside a single rule**. This is component composition: build pieces, then snap them together.

## Composing a button from pieces

Define focused building blocks:

```fscss
@define btn-base(pad: 0.75rem 1.5rem) {
  padding: @use(pad);
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

@define btn-primary(bg: #3b82f6) {
  background: @use(bg);
  color: white;
}

@define btn-ghost(fg: #64748b) {
  background: transparent;
  color: @use(fg);
  border: 1px solid currentColor;
}

.primary-btn {
  @btn-base()
  @btn-primary()
}

.ghost-btn {
  @btn-base(1rem 2rem)
  @btn-ghost()
}
```

Compiles to:

```css
.primary-btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: #3b82f6;
  color: white;
}

.ghost-btn {
  padding: 1rem 2rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: transparent;
  color: #64748b;
  border: 1px solid currentColor;
}
```

One base + one flavor = two buttons. Add a third flavor without touching the base.

## Composition even works on plain rules

You can also compose directly onto plain CSS declarations:

```fscss
@define reset(st) {
  @use(st), @use(st) * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
}

@reset(.widget)
```

This mirrors the pattern used by `flux-base` in `flux-wave.fscss` where a scoped reset is injected into a component.

## The `flux-wave.fscss` composite pattern

Real libraries wrap multiple defines into one "preset" define. This is the exact structure used by `flux-wave.fscss`:

```fscss
@define flux-wave-preset(st:.wave-container, bandCount: flux-i){
  @flux-base(@use(st))
  @flux-container(@use(st))
  @flux-band(@use(st) .wave)
  @flux-bands(@use(st) .wave, @use(bandCount))
}
```

And consumers call one thing:

```fscss
@flux-wave-preset(.wave-container)
```

This is the payoff of composition: a complex component becomes a single, readable call.

## Rules of composition

1. **Keep defines single-purpose** — one concern per define.
2. **Order matters** — later defines can override earlier ones (cascade still applies).
3. **Pass selectors, not just values** — this is how defines customize each other.
4. **Provide a "preset" define** that composes the full component with sane defaults.

## Exercise

Compose an alert component from a `@alrt-base` (padding, radius, border), `@alrt-variant(kind)` using `@event` (Tutorial 15) or parameter switches, and wire three variants: `info`, `warn`, `error`.

## Key takeaways

- Apply many defines in one ruleset.
- Compose value-defines + selector-defines into full components.
- Provide one-call presets for complex components.

## Next

[Block defines & media generation](09-block-defines-and-media/README.md)