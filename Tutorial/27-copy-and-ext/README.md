# Tutorial 27 — Substring Tools: `copy()` and `@ext()`

Both slice substrings from values — `copy()` stores the slice into a variable; `@ext()` stores it into a named reference you read back with `@ext.name`.

## `copy()` — capture a substring as a variable

```fscss
body {
  background: #4ff000 copy(4, primary-color);
  color: $primary-color!;
}
```

Compiles to:

```css
body {
  background: #4ff000;
  color: var(--primary-color);
}
:root { --primary-color: #4ff; }
```

What happened:
1. `copy(4, primary-color)` — take the first 4 characters of the value context (`#4ff`).
2. Store under variable name `primary-color`.
3. Later `$primary-color!` reads it back.

A positive length counts from the start; a negative length counts from the end:

```fscss
// copy(3, tail)  -> last 3 characters
```

A length greater than the string returns the whole string.

## `@ext()` — slice by index

```fscss
body {
  property: "the red color @ext(4,3: myRed)";
  color: @ext.myRed;
}
```

Compiles to:

```css
body {
  property: "the red color";
  color: red;
}
```

What happened:
- `@ext(4,3: myRed)` — inside the string, start at index 4 and take 3 characters (`red`), storing the result as `myRed`.
- The original slice is cut from the output text.
- `@ext.myRed` returns `red` elsewhere.

## Practical: extracting hex channels

```fscss
body {
  background: #aabbcc @ext(1,2: r)";"; @ext(3,2: g)";"; @ext(5,2: b)";";
  // compile-time channel pulls — illustrative pattern
}
```

When extracting from the *middle* of a literal like a hex color, keep the string approach:

```fscss
$hex: "#ff5522";
component { decoration: "@ext(1,3: hot)"; }
.hot swatch { background: #ff5522; }
```

Real usage tends to be on tokens you want to reuse text from rather than raw colors.

## Summary matrix

| Tool | Slice source | Store | Read back |
|---|---|---|---|
| `copy(len, name)` | current value | variable `name` | `$name!` |
| `@ext(start, len: name)` | string in place | named extract `name` | `@ext.name` |

## Gotchas

- `copy()` lengths can be negative (from the end) or oversized (full string).
- `@ext()` offsets are 1-based indexes into the string.
- Output text is trimmed of the sliced span; the extracted value is reusable via its name.

## Exercise

Use `copy()` to shorten the hex `#336699` into a `--short` brand token (`#336`), and `@ext()` to carve `blue` out of `"bright blue sky"` and reuse it as a color.

## Key takeaways

- `copy(len, name)` → save a slice into a variable.
- `@ext(start, len: name)` → cut a slice by index, reuse as `@ext.name`.
- The string toolkit behind token-derivation workflows.

## Next

[rpt() & pattern()](28-rpt-and-pattern/README.md)