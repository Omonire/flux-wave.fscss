# Tutorial 20 — Shared-Value Shorthand `%n()`

The `%n()` family is FSCSS's classic shorthand: apply one value to many properties in a single call. `%2` through `%6` cover the common counts; `%i` handles a custom count.

## Basic usage

```fscss
div {
  %2(width, height[: 50px;])
}

.box {
  %3(border-radius, outline-width, min-width[: 5px;])
}
```

Compiles to:

```css
div {
  width: 50px;
  height: 50px;
}

.box {
  border-radius: 5px;
  outline-width: 5px;
  min-width: 5px;
}
```

The `[: value;]` block carries the value.
- `%2(a, b[: value;])` → 2 properties, one value
- `%3(...)` → 3 properties, one value
- …up to `%6`

## `%i` for custom counts

`%i` takes any number of properties:

```fscss
.panel {
  %i(margin, padding, border, background, min-height[: 0;])
}
```

## The full supported range

| Call | Generates |
|---|---|
| `%2(a, b[val])` | `a: val; b: val;` |
| `%3(a, b, c[val])` | three properties with the same value |
| `%4` / `%5` / `%6` | four/five/six properties |
| `%i(a, b, c, d, e, f, g[val])` | any count with the same value |

## Practical: aspect-ratio-free squares and equal sides

```fscss
.avatar {
  %2(width, height[: 48px;])
  border-radius: 50%;
}

.icon-box {
  %2(width, height[: 24px;])
  %2(display, align-items[: flex;])   /* tricksy but legal with proper values */
}
```

Keep it clean — `%2` shines for dimension pairs:

```fscss
.spinner {
  %2(width, height[: 32px;])
  border: 3px solid #ddd;
  border-top-color: #3b82f6;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}
```

## How values are written

The value block is literally `[: <value>;]` — colon, value, semicolon inside brackets. This is why `mx()` (Tutorial 21) exists too: it's the more flexible sibling.

## Gotchas

- The bracket value must include `: ` and `;` — copy the shape exactly.
- `%2`–`%6` are fixed-width; use `%i` for other counts.
- All properties receive the exact same value; no per-property customization.

## Exercise

Write a `.page-loading` placeholder that is a perfect square of `120px`, plus a `.sr-only`-like zero group using `%i`.

## Key takeaways

- `%n(props[: value;])` applies one value to n properties.
- `%2`–`%6` fixed; `%i` for custom counts.
- The grandparent of `mx()`/`mxs()`.

## Next

[mx() and mxs()](21-mx-and-mxs/README.md)