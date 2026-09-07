# Tutorial 23 — Compact Keyframes

`$(@keyframes name, selectors, &[duration timing options])` defines a `@keyframes` rule *and* applies the resulting animation to the given selectors — all in one block.

## The full form

```fscss
$(@keyframes slideIn, .box, .card, &[3s linear infinite]) {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}
```

Compiles to:

```css
.box, .card {
  animation: slideIn 3s linear infinite;
}
@keyframes slideIn {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}
```

Two outputs from one block:
1. The `animation:` declaration on the listed selectors, using `&[3s linear infinite]` for timing.
2. The standalone `@keyframes slideIn` rule.

## Multiple keyframe stops

Use percentages exactly as in CSS:

```fscss
$(@keyframes pulse, .badge, &[1.2s ease-in-out infinite]) {
  0%   { transform: scale(1); opacity: 1; }
  50%  { transform: scale(1.15); opacity: 0.7; }
  100% { transform: scale(1); opacity: 1; }
}
```

## Applying to multiple selectors

The selectors list is comma-separated before the timing block:

```fscss
$(@keyframes shake, .btn-error, .input-error, &[0.5s linear both]) {
  from, to { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}
```

## Inside `@define` for component packaging

Pair with `@define` (Tutorial 09) to ship animation + styles in one component:

```fscss
@define spin-coin(sel: .coin, dur: 2s) {
  `
  @use(sel) {
    @fun.coin-base
  }
  $(@keyframes flip, @use(sel), &[@use(dur) ease-in-out infinite]) {
    0% { transform: rotateY(0); }
    100% { transform: rotateY(360deg); }
  }
  `
}
```

## Gotchas

- The `&[...]` timing block is mandatory for the animation declaration output.
- If you need multiple animations, prefer separate compact blocks or the classic form.
- Everything compiles to standard `@keyframes` + `animation:` — fully compatible.

## Exercise

Create a floating "go to top" button: define `$(@keyframes bob, .to-top, &[3s ease-in-out infinite])` with a gentle translateY bounce.

## Key takeaways

- `$(@keyframes name, selectors..., &[timing]) { ... }` = animation declaration + keyframes.
- Selectors get `animation: name timing;`.
- Compose with `@define` for packaged components.

## Next

[Vendor prefixing](24-vendor-prefixing/README.md)