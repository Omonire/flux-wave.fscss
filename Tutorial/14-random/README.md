# Tutorial 14 — Randomness with `@random`

`@random()` picks one value from a list — either inline or from an array — at compile time (or at each render in runtime mode). It brings "dynamic, ever-changing styles" to CSS without a single line of JavaScript.

## Inline list

```fscss
.btn {
  background: @random([red, blue, green, brown]);
  transform: translate(@random([10, 30, 60])px);
  rotate: @random([0, 90, 150])deg;
}
```

Every `@random()` call is independent, so one rule can randomize several properties separately.

## From a declared array

```fscss
@arr(palette[#4361ee, #f72585, #7209b7]);

.btn {
  border: 2px groove @random(@arr.palette);
}
```

Values come from the array; the result is a single item.

## Compile-time vs render-time

- **CLI/static compile:** each compilation picks one value per `@random()` call. Recompile to re-randomize.
- **Runtime mode (browser):** each page render can select a new value → different visuals per visit.

That difference is a feature: static builds stay deterministic, demos stay lively.

## Practical: randomized decorative backgrounds

```fscss
@arr(grads[
  linear-gradient(135deg, #667eea, #764ba2),
  linear-gradient(135deg, #f093fb, #f5576c),
  linear-gradient(135deg, #4facfe, #00f2fe)
]);

.hero-card {
  background: @random(@arr.grads);
}
```

## Combining with `num()` for shifted values

```fscss
.card {
  top: num(@random([0, 5, 10]) * 1)px;        /* 0, 5, or 10 */
  opacity: num(@random([100, 80, 60]) / 100); /* 1, 0.8, or 0.6 */
}
```

Or splice randomness directly into a formula like the docs' example:

```fscss
textarea {
  max-height: num(@random([40, 10, 5, 0]) + 50);
}
```

## Where randomness shines

- Sprite/pattern variation for tileable backgrounds
- Confetti and burst particle delays
- Decorative avatars (pick a hue per render)
- Test-data UIs that re-randomize each reload

## Gotchas

- `@random([...])` needs the full list; `@random(@arr.name)` needs a declared array.
- It's a *pick*, not a shuffle of many — one value per call.
- For multiple different values in one rule, call `@random` multiple times.

## Exercise

Build a "confetti dot field": 5 absolutely-positioned dots whose `left`, `background`, and `animation-delay` are each randomized via `@random` over arrays.

## Key takeaways

- `@random([a, b, c])` or `@random(@arr.name)` → one random item.
- Re-evaluates per compile/render.
- Combine with `num()` and arrays for endless variations.

## Next

[Conditional logic with @event](15-event-logic/README.md)