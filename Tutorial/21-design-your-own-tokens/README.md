# Tutorial 21 — Design Your Own Tokens

Now you build. The first piece to re-create is `flux-tokens` — the block `@define` that writes custom properties *and* an `@keyframes` in one shot.

## Study the original

```fscss
@define flux-tokens(root:root){`
  :@use(root){
    --flux-wrap-width: 100%;
    --flux-wrap-max-width: 560px;
    --flux-wrap-height: 160px;
    --flux-wrap-bg: #000;
    --flux-wrap-radius: 28px;
    ...
  }

  @keyframes flux-flow {
    0% { transform: translateX(-8%) scaleY(0.55) skewX(-4deg); }
    ...
  }
`}
```

Three FSCSS ideas inside one mixin:

1. **Parameterized selector:** `(root:root)` — the target defaults to `:root`.
2. **Backtick block define:** the whole body is a string block (Tutorial 22 details these).
3. **Two outputs:** property declarations **and** a standalone `@keyframes`.

## Build your own "glow-tokens"

```fscss
@define glow-tokens(root:root){`
  :@use(root){
    --glow-size: 120px;
    --glow-bg: #0b0b18;
    --glow-radius: 50%;
    --glow-c1: #7dd3fc;
    --glow-c2: #93c5fd;
    --glow-dur: 2.4s;
  }

  @keyframes glow-pulse {
    0%   { transform: scale(1);   opacity: 0.6; }
    50%  { transform: scale(1.25); opacity: 1; }
    100% { transform: scale(1);   opacity: 0.6; }
  }
`}
```

Call with `@glow-tokens()`. Both the `:root` custom properties and `glow-pulse` keyframes are emitted — the exact shape of `flux-tokens`.

## Why tokens, really

Decoupling data from styling means:

- Consumers override `--glow-dur` instead of hunting for a hardcoded `2.4s`.
- Every component rule reads `var()` with fallbacks, so missing pieces degrade gracefully.
- One mixin writes a consistent baseline for the whole component family.

## The three-layer fallback ladder (copy this habit)

When you write style rules, give every custom property a fallback *and* a generic shared default where sensible:

```fscss
background: var(--glow-c2, var(--glow-c1, #93c5fd));
```

Specific → shared → literal. That's the belt-and-suspenders pattern flux-wave uses everywhere.

## Naming your tokens

Follow the prefix convention: `--<component>-<group>-<prop>`. flux-wave uses `--flux-wrap-*`, `--flux-band-*`, `--flux-bandN-*`. Yours could be `--glow-*` as above. Consistent prefixes make overrides auto-documenting.

## Checkpoint

- `@define name(root:root){ \` :root {...} @keyframes {...} \` }`.
- Token mixins emit custom properties + keyframes together.
- Always provide fallbacks in rule reads.

Next — [22 · Write your band mixins](22-write-your-band-mixins/README.md)