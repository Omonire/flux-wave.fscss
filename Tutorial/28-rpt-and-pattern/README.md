# Tutorial 28 — Repetition `rpt()` & Semantic Matching `pattern()`

Two very different features: `rpt()` repeats a value; `pattern()` matches *natural-language descriptions* and injects CSS when the similarity passes a threshold.

## rpt() — repeat a value

Most commonly used inside `content` for decorative or generated text:

```fscss
.separator::after {
  content: "rpt(10, '— ')";
}
```

Compiles to:

```css
.separator::after {
  content: "— — — — — — — — — — —";
}
```

`rpt(count, value)` — repeat `value` a given number of times. Useful for:

- Fancy separators and rules
- Placeholder patterns
- Progress-style glyph art in `content`

## pattern() — semantic matching

`pattern()` matches a natural-language description against phrases written elsewhere in a stylesheet and injects the associated CSS when the similarity score meets a defined threshold.

### Syntax

```fscss
pattern(threshold: "description", `
  css
`)
```

- `threshold` — `0` to `1`, minimum similarity required to trigger. Defaults to `1` (near-exact) if omitted.
- The second argument is a CSS block (backticks).

### Example

```fscss
pattern(0.5: "Hello World card", `
  border-radius: 12px;
  padding: 24px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #fff;
`)

.card {
  hello world card
}
```

Compiles to:

```css
.card {
  border-radius: 12px;
  padding: 24px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #fff;
}
```

The bare phrase `hello world card` inside `.card` matches the registered description at or above 0.5 similarity, so the block is injected.

### Property patterns & block patterns

Two flavors:

- **Property pattern** — injects a set of declarations *inside* a selector (above example).
- **Block pattern** — resolves to a full structure like a `@keyframes` and may be written standalone, outside any selector:

```fscss
pattern(0.7: "animated keyframe for spin", `
  @keyframes spin {
    0% { transform: rotate(0); }
    100% { transform: rotate(360deg); }
  }
`)

an Animated keyframe for spin
```

## How thresholds behave

| Threshold | Meaning |
|---|---|
| `1` (default) | near-exact text match required |
| `0.5` | roughly half the words must match |
| `0` | matches almost anything |

Tune lower thresholds for fuzzy "idea" matching; keep high for deterministic behavior.

## Practical: documenting intent in classes

Some designers love that the phrase *reads like English*:

```scss
pattern(0.6: "rounded ghost button", ` ... `)
.cta-secondary { rounded ghost button }
```

## Gotchas

- `pattern()` resolves by approximate meaning, not exact names — predictable when the phrase is distinctive.
- Threshold too low → accidental injections; too high → behaves like a strict lookup.
- Availability: `pattern()` requires v1.1.25+ (CDN, API, CLI).

## Exercise

Use `pattern()` to register a "soft shadow card" block at 0.5 and trigger it with a slightly different phrase, then register a block `@keyframes` for "gentle float" and trigger it standalone.

## Key takeaways

- `rpt(n, 'v')` → repeated text in `content`.
- `pattern(threshold: "phrase", \`css\`)` → semantic injection.
- Thresholds trade determinism for flexibility.

## Next

[exec() debugging & pipeline control](29-exec-debugging/README.md)