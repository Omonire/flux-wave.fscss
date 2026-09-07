# Tutorial 11 — Arrays with `@arr`

Arrays are where FSCSS gets genuinely dynamic. `@arr` stores ordered collections of values that you can reference by index or iterate automatically — perfect for staggered animations, theming scales, and generated utilities.

## Declaration & access

```fscss
@arr(colors[#3b82f6, #8b5cf6, #06b6d4, #10b981]);
@arr(spacing[0.5rem, 1rem, 1.5rem, 2rem]);

.primary-button {
  background: @arr.colors[1];
  padding: @arr.spacing[2] @arr.spacing[3];
}
```

Compiles to:

```css
.primary-button {
  background: #3b82f6;
  padding: 1.5rem 2rem;
}
```

Key facts to internalize:

- **1-indexed** — the first item is `[1]`, not `[0]`.
- String-native — every array access returns a string (you use the result directly in CSS).
- Method calls do **not** chain (covered in Tutorial 12).

## The `count()` shortcut

You can construct arrays on the fly with `count()`:

```fscss
@arr(indexes[count(4, 1)]);      /* 1, 2, 3, 4 */
@arr(delays[count(9, 0.5)]);     /* 0.5, 1.5, ... */  (numeric series)
```

Combined with event delay styling this powers stagger effects without writing each nth-child by hand.

## Direct output mode & method mode

There are two ways to touch an array:

```fscss
@arr.name        /* direct output: item1, item2, item3 */
@arr.name!.method /* method access mode (Tutorial 12) */
```

The `!` after the array name switches to method access mode.

## Practical: theme palette by index

```fscss
@arr(palette[#ef4444, #f59e0b, #10b981, #06b6d4, #8b5cf6]);

.swatch.swatch-1 { background: @arr.palette[1]; }
.swatch.swatch-2 { background: @arr.palette[2]; }
.swatch.swatch-3 { background: @arr.palette[3]; }
.swatch.swatch-4 { background: @arr.palette[4]; }
.swatch.swatch-5 { background: @arr.palette[5]; }
```

You'd rarely write that by hand — instead you combine with auto-indexing (Tutorial 13).

## Array mutation

You can grow or shrink a declared array with index operators:

```fscss
@arr(name!+[item4, item5]);   /* append items */
@arr(name!-[2]);              /* remove the second item */
```

Note the `!` — mutation APIs are part of method access mode.

## Limitations (be aware early)

- **No chaining** — each method call returns a plain string, not another array.
- **No nested arrays** — `@arr(b[@arr(a!.list)])` stores the literal text; inner arrays aren't executed.

## Exercise

Declare a `@arr(breakpoints[600px, 900px, 1200px])` and average... no — use it to build three `@media`-anchored rules by index.

## Key takeaways

- `@arr(name[...])` declares; `@arr.name[i]` indexes (1-based).
- Building arrays from `count()` enables generated sequences.
- Keep in mind: strings in, strings out, no chaining.

## Next

[Array methods](12-array-methods/README.md)