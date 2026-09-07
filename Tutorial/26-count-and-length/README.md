# Tutorial 26 — `count()` and `length()`

Two small utilities that feed the bigger features:

- `count(limit)` / `count(limit, step)` — sequential numeric values.
- `length("text")` — character count of a string.

## `count()` basics

```fscss
exec(_log, "count(5)")     /* 1, 2, 3, 4, 5 */
exec(_log, "count(10, 2)") /* 2, 4, 6, 8, 10 */
```

- One argument → `1, 2, ..., limit`.
- Two arguments → start at the *step* value and continue by adding step until reaching `limit`.

## count() driving arrays & nth-child

This is the classic pairing — generate an index array, then auto-iterate:

```fscss
@arr num[count(5)]
div:nth-child(@arr.num[]) {
  animation-delay: @arr.num[];
}
```

Compiles to five rules whose `animation-delay` is `1s, 2s, 3s, 4s, 5s` respectively.

## Staggered delays with step

```fscss
@arr(ticks[count(9, 1)]);

.dot:nth-child(@arr.ticks[]) {
  $i: @arr.ticks[];
  animation-delay: num($i! * 0.1)s;   /* 0.1s, 0.2s, ... 0.9s */
}
```

Step + math = smooth stagger sequences without blinking a lone variable.

## `length()` basics

```fscss
.length-demo {
  width: num(length("Hello World") * 10)px;
}
```

Compiles to:

```css
.length-demo {
  width: 110px;
}
```

`"Hello World"` is 11 characters (space counts) × 10 = 110px.

## Practical: proportional badge by text length

```fscss
@fun(len-map){
  short: num(length("OK") * 8)px;          /* 16px */
  medium: num(length("Accept terms") * 8)px; /* 96px */
  long: num(length("Confirm your email now") * 8)px;
}
```

## Combining with num() — dynamic boxes

```fscss
.label {
  min-width: num(length(attr(data-count)) * 7)px;
}
```

(In two-step state: `attr()` is runtime CSS; `length()` + `num()` are compile-time — use them on literal strings or variables, not attribute values.)

## Gotchas

- `length()` counts every character including spaces and punctuation.
- `count(limit, step)` starts *at* step, not 1.
- Feed the results into `num()` for math.

## Exercise

Build a `<ol>` with 7 items where each item gets `opacity: num(1 - $i! * 0.1)` via `count(7,1)`, and a tag pill whose padding scales with `length()` of a literal string.

## Key takeaways

- `count(n)` → `1..n`; `count(n, s)` → stepped sequence.
- `length("text")` → character count.
- Both plug directly into `num()` and arrays.

## Next

[Substring tools: copy() & @ext()](27-copy-and-ext/README.md)