# Tutorial 16 — Comparisons & Math Inside Events

`@event` conditions are not limited to exact string matches. Numeric parameters support **operators** (`==`, `>`, `<`, `>=`, `<=`) and branch values can run through **`num()`** math.

## Numeric conditions

```fscss
@event rating(score) {
  if score >= 90 {
    return: #10b981;
  }
  el-if score >= 70 {
    return: #f59e0b;
  }
  el {
    return: #ef4444;
  }
}

.user-score-95 { color: @event.rating(95); }
```

Compiles to:

```css
.user-score-95 {
  color: #10b981;
}
```

95 hits the first branch; 80 would hit the second; 50 the `el`.

## Comparison operators

| Operator | Meaning |
|---|---|
| `==` | equal to |
| `>` | greater than |
| `<` | less than |
| `>=` | greater than or equal |
| `<=` | less than or equal |

Mix them across branches:

```fscss
@event muffin-size(oz) {
  if oz < 2  { return: mini; }
  el-if oz <= 9 { return: normal; }
  el     { return: jumbo; }
}
```

## Math inside branches with `num()`

```fscss
@event spacing(level) {
  if level == 1 {
    return: num(4*2)px;
  }
  el-if level == 2 {
    return: num(8*2)px;
  }
  el {
    return: num(16*2)px;
  }
}

.card-medium { padding: @event.spacing(2); }
```

Compiles to:

```css
.card-medium {
  padding: 16px;
}
```

## Combining everything: a progress color + size system

```fscss
@event progressStyle(p) {
  if p >= 90 { return: #10b981; }
  el-if p >= 50 { return: #f59e0b; }
  el { return: #ef4444; }
}

@event progressWidth(p) {
  return: num(@use(p) * 1)%;
}

.bar {
  background: @event.progressStyle(85);
}
.bar.small { width: num(85 * 1)%; }
```

Better — compute the width for the current value directly in the app rule:

```fscss
.bar { width: num(@event.progressWidth(85))%; }
```

## Building derived scales

Because branches can return computed sizes, you can define whole derived scales from a single parameter:

```fscss
@event typeScale(step) {
  if step == 1 { return: num(1 * 1.2)rem; }
  if step == 2 { return: num(1.2 * 1.2)rem; }
  if step == 3 { return: num(1.44 * 1.2)rem; }
  el { return: 1rem; }
}
```

## Gotchas

- Comparisons work on numeric parameters — pass numbers at the call site.
- Wrap the branch output in `num()` before math.
- `@use(p)` is not needed inside event bodies unless nested in computed output; plain `return:` values also accept `num()` directly.

## Exercise

Build `@event heatMap(value)`: `>= 75` → hot red gradient shade, `>= 50` → amber, `else` → green. Then emit 5 swatch rules for values 90, 70, 60, 30, 10.

## Key takeaways

- Use `== > < >= <=` in `if`/`el-if` for numeric parameters.
- Combine returns with `num()` for computed values.
- Events become small, readable design-language rules.

## Next

[Imports — the basics](17-import-basics/README.md)