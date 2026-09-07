# Tutorial 24 — Automatic Vendor Prefixing

The `-*-` prefix expands a property across `-webkit-`, `-moz-`, `-ms-`, and `-o-`, followed by the unprefixed property.

## Basic usage

```fscss
.box {
  -*-transform: rotate(45deg);
}
```

Compiles to:

```css
.box {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

Four prefixed declarations plus the standard one — no copy-paste, no constructor tool.

## Which properties benefit?

Classic candidates that still want prefixes defensively:

- `transform`
- `transition`
- `animation`
- `filter`
- `backdrop-filter`
- `appearance`
- `user-select`

```fscss
.card-3d {
  -*-perspective: 800px;
  -*-transform-style: preserve-3d;
}

.native-slider {
  -*-appearance: none;
}
```

## Combination with values

The value follows normally after the colon:

```fscss
.fade-tab {
  -*-transition: opacity 0.3s ease;
}
```

Compiles to `-webkit-transition: ...; -moz-transition: ...; -ms-transition: ...; -o-transition: ...; transition: ...;`

## Inside shorthand-heavy rules

Vendor expansion works anywhere a property is declared:

```fscss
.hero {
  -*-transform: translate3d(0,0,0);
  will-change: transform;
}
```

## Gotchas

- Only properties that genuinely need prefixes should use `-*-` (modern browsers rarely need all four — but harmless when present).
- Keep the unprefixed *standard* property last (FSCSS does this for you).
- Works best on declaration values that are identical across prefixes, which is true for the listed properties.

## Exercise

Prefix `appearance` and `backdrop-filter` in a `.glass-pill` component, then inspect the compiled output to confirm all four vendor variants + standard.

## Key takeaways

- `-*-prop: value;` → webkit/moz/ms/o + standard.
- Zero-config vendor support for the often-needed properties.

## Next

[Math with num()](25-num-calculations/README.md)