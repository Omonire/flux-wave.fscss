# Tutorial 21 — `mx()` and `mxs()`

`mx()` and `mxs()` both apply one value to many properties, but differ in how you write the shared value:

- `mxs(prop1, prop2, ..., 'plain value')` — plain shared value.
- `mx(prop1, prop2, ..., ': value;')` — explicit colon+semicolon, like `%n`.

`mx()` is the more flexible form: because you write `: value;` explicitly, you can slip in anything that looks like a declaration fragment.

## mxs — plain value

```fscss
.card {
  mxs(width, height, max-height, max-width, min-width, min-height, '200px')
}
```

Compiles to:

```css
.card {
  width: 200px;
  height: 200px;
  max-height: 200px;
  max-width: 200px;
  min-width: 200px;
  min-height: 200px;
}
```

## mx — explicit declaration fragment

```fscss
.box {
  mx(width, height, max-height, max-width, min-width, min-height, ': 200px;')
}
```

Same output — but `mx` also supports trickier fragmented values.

## Which to use?

- Prefer `mxs` for everyday "same simple value everywhere."
- Use `mx` when your value needs to carry special syntax, like a CSS function with its own punctuation:

```fscss
.responsive {
  mx(max-width, width, ': min(600px, 90vw);')
}
```

## Pairing with box utilities

```fscss
.box-chip {
  mxs(border-radius, ... )
}

.square-tile {
  mxs(width, height, '64px')
  mxs(top, left, '0')
  mxs(display, align-items, justify-content, '...')   /* careful: values must be valid per property */
}
```

## Gotchas

- `mxs` takes a plain string value (no colon/semicolon).
- `mx` requires the `: value;` fragment shape.
- You can't give different values per property — these are *shared-value* tools.

## Exercise

Style a `.thumb` that is a `96px` square with equal `max-/min-` boxing using a single `mxs`, then refactor a rule that repeats `0` for `top`, `right`, `bottom`, `left` into one `mxs`.

## Key takeaways

- `mxs(props..., 'value')` — plain shared value.
- `mx(props..., ': value;')` — explicit fragment, more flexible.
- Perfect for square/box/reset utilities.

## Next

[Attribute selector shorthand](22-attribute-selectors/README.md)