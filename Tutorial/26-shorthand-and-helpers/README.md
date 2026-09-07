# Tutorial 26 — Shorthand & Helpers

FSCSS shorthand (`%n`, `mxs`, `mx`, vendor `-*-`) lets you write the *plumbing* around waves in much less space — utilities, reset flip, centered helpers, prefixed props.

## %n — shared-value inline

Give width and height one shot:

```fscss
.badge-size {
  %2(width, height[: 96px;])
}
```

Compiles to:

```css
.badge-size { width: 96px; height: 96px; }
```

Inside your `@define` shells, use it to keep shell mixins tight:

```fscss
@define badge-base(st: .badge){`
  @use(st){
    %2(width, height[: var(--badge-size, 96px);])
    ...
  }
`}
```

## mxs — multi-property same value

Reset a group to zero in one line:

```fscss
@use(st), @use(st) * {
  mxs(margin, padding, '0')
  box-sizing: border-box;
}
```

That's the entire `flux-base` reset written as shorthand — fewer keystrokes, same output.

## mx — explicit fragment form

```fscss
@use(st) {
  mx(max-width, width, ': min(600px, 90vw);')
}
```

Useful when the value carries its own punctuation (`min(...)` functions).

## Vendor prefixing — -*- 

Flux waves belong on all browsers; add prefixes without typing them:

```fscss
.wave-container .wave {
  -*-transform: translateX(-8%) scaleY(0.55) skewX(-4deg);
  -webkit-filter: blur(12px);
}
```

Wait — `-*-transform` expands all four vendor variants. But `flux-flow` is a *keyframe name*; the property decks inside keyframes are also candidates:

```fscss
@keyframes flux-flow {
  0%   { -*-transform: translateX(-8%) scaleY(0.55) skewX(-4deg); }
  ...
}
```

Expands to `-webkit-transform/-moz-transform/...` + standard inside each keyframe.

## Building clean helpers with %n

```fscss
@define square(st, s){`
  @use(st){
    %2(width, height[: @use(s);])
    flex: none;
  }
`}
```

Call it inside wave pages for icons, dots, avatars:

```fscss
@square(.icon, 24px)
@square(.avatar, 40px)
```

## When NOT to shorthand

- Readability first: `%2(width, height[…])` is great; a 20-property `%i` is a code smell.
- Keep shorthand inside helpers/mixins, not scattered across your theme file.
- Values with unrelated units per property (e.g. width in px, height in %) still use plain declarations.

## Checkpoint

- `%2(props[: value;])`, `mxs(props, 'value')`, `mx(props, ': frag;')`.
- `-*-prop` → vendor-expanded + standard.
- Use shorthand for helpers and resets; keep declarations readable.

Next — [27 · Responsive & accessible](27-responsive-and-accessible/README.md)