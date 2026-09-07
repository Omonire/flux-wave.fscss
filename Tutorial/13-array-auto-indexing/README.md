# Tutorial 13 — Array Auto-Indexing

The empty-bracket reference `@arr.name[]` is FSCSS's loop. When used in a selector (like `:nth-child()`), it iterates through every item and generates one rule per item. This is how you generate stagger animations and repeating utilities without typing each one.

## The loading dots pattern

```fscss
@arr(delays[0.1s, 0.3s, 0.5s]);
@arr(colors[#ef4444, #f59e0b, #10b981]);
@arr(indexes[count(3, 1)]);

.loading-dot:nth-child(@arr.indexes[]) {
  $index: @arr.indexes[];
  animation-delay: @arr.delays[$index!];
  background: @arr.colors[$index!];
}
```

Compiles to:

```css
.loading-dot:nth-child(1) {
  animation-delay: 0.1s;
  background: #ef4444;
}
.loading-dot:nth-child(2) {
  animation-delay: 0.3s;
  background: #f59e0b;
}
.loading-dot:nth-child(3) {
  animation-delay: 0.5s;
  background: #10b981;
}
```

Two arrays drive three rules. Add a fourth dot → add one value to each array → done.

## How the pieces fit

1. `@arr(indexes[count(n, 1)])` — the *drive array*: yields `1, 2, 3, ...`.
2. `:nth-child(@arr.indexes[])` — the empty brackets iterate the drive array, producing one rule per index.
3. Inside the rule, `$index: @arr.indexes[];` captures the current index.
4. `@arr.delays[$index!]` uses that index to pull the matching item from the *value arrays*.

Current index captured as `$index!` and reused as an array index — that's the loop body.

## Alternating styles example

```fscss
@arr(idx[count(4, 1)]);
@arr(barColors[#22c55e, #3b82f6, #a855f7, #ef4444]);

li:nth-child(@arr.idx[]) {
  $i: @arr.idx[];
  border-left: 4px solid @arr.barColors[$i!];
}
```

## Real-world: the flux-wave band generation

This is exactly how `flux-wave.fscss` generates its four gradient bands:

```fscss
@arr flux-i[count(4,1)]
@use(st).wave-@arr.@use(counter)[]{
  background: var(--flux-band@arr.@use(counter)[]-bg, none);
  top: var(--flux-band@arr.@use(counter)[]-top, 30%);
  height: var(--flux-band@arr.@use(counter)[]-height, var(--flux-band-height, 80px));
  ...
}
```

A `count(4,1)` array expands into `wave-1`, `wave-2`, `wave-3`, `wave-4`, each pulling its own token (`--flux-band2-bg`, etc.). Four bands, one block of code.

## Rules for auto-indexing

- The drive array **must** be usable in the selector position (`:nth-child`, class suffix, etc.).
- Keep value arrays in sync with the drive array length.
- `$index!` inside the block gives you the number to index other arrays.
- Don't forget 1-indexing everywhere.

## Exercise

Build an animated progress "ticks" row: 6 bars whose heights and delays come from two value arrays driven by `@arr(idx[count(6,1)])`.

## Key takeaways

- `@arr.name[]` in a selector = loop → one rule per item.
- Capture the current index and use it to index value arrays.
- This is the engine behind staggered, generated CSS.

## Next

[Randomness with @random](14-random/README.md)