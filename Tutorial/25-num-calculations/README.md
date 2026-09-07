# Tutorial 25 — In-Stylesheet Math with `num()`

`num(expression)` evaluates arithmetic at compile time, supporting `+ - * /`. A unit may be appended directly after the closing parenthesis.

## Basic usage

```fscss
selector {
  max-height: num(40 * 4);
}
```

Compiles to:

```css
selector {
  max-height: 160;
}
```

## Appending units

```fscss
.sidebar {
  width: num(100 - 30)px;      /* 70px */
  padding: num(8 * 2)px;       /* 16px */
}

.container {
  max-width: num(1200 / 2);
}
```

## Randomness + math (from the docs)

```fscss
textarea {
  max-height: num(@random([40, 10, 5, 0]) + 50);
}
```

Output is `90`, `60`, `55`, or `50` depending on the pick. `@random` results flow straight into math.

## Building scales with math

```fscss
@arr(step[count(3, 1)]);
$base: 16px;

.rungs nth-child(@arr.step[]) {
  $n: @arr.step[];
  font-size: num($base! * $n!);
}
```

(Pairing `num()` with `$n!` from an array drive — the loop-power of Tutorial 13 combined with math.)

## Practical: fluid-ish grid columns

```fscss
.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, num(100% / 12));
}
```

Or compute a quarter-width drawer:

```fscss
.drawer {
  width: num(100% / 4);        /* unitless: not ideal — better written as 25% */
}
```

> **Note on percentages:** `num()` produces plain numbers; append the unit you need (`%;`, `px`, `rem`) after the call when the context requires one.

## Combining with events (Tutorial 16)

```fscss
@event gap(level) {
  if level == 1 { return: num(4 * 1)px; }
  el-if level == 2 { return: num(4 * 2)px; }
  el { return: num(4 * 3)px; }
}

.grid { gap: @event.gap(2); }   /* 8px */
```

## Gotchas

- Operators: `+ - * /` (and parentheses).
- Append units outside the call: `num(x * y)px`.
- `num()` output is unitless unless you add the unit yourself.

## Exercise

Create a 3-card layout where each card width is `num((100% - 2*gap) / 3)` once gaps are declared as variables, and a `$font-base * 1.25` heading size.

## Key takeaways

- `num(expr)` = compile-time math.
- Unit appended outside the parens.
- Composes with `@random`, arrays, and `@event`.

## Next

[count() and length()](26-count-and-length/README.md)