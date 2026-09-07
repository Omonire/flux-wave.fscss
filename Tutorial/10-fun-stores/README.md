# Tutorial 10 — Function Stores with `@fun`

`@fun(name){ ... }` defines a named group of key–value pairs, referenced with dot notation. It is purpose-built for **design tokens**: spacing scales, color palettes, typography tiers.

## Full block use

```fscss
@fun(card-style){
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.card {
  @fun.card-style
  border: 1px solid #e2e8f0;
}
```

Compiles to:

```css
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  border: 1px solid #e2e8f0;
}
```

## Access levels

| Expression | What you get |
|---|---|
| `@fun.name` | The full block of properties |
| `@fun.name.property` | One property pair: `property: value;` |
| `@fun.name.property.value` | Just the value, for inline use (gradients, calc, etc.) |

## Property & value access

```fscss
@fun(e){
  a: 100px;
  b: 200px;
}
@fun(col){
  1: #550066;
  2: #005523;
}

.box {
  width: @fun.e.a.value;
  height: @fun.e.b.value;
  background: linear-gradient(@fun.col.1.value, @fun.col.2.value);
}
```

Compiles to:

```css
.box {
  width: 100px;
  height: 200px;
  background: linear-gradient(#550066, #005523);
}
```

## A real design-token store

```fscss
@fun(space){
  xs: 0.25rem;
  sm: 0.5rem;
  md: 1rem;
  lg: 2rem;
  xl: 3rem;
}

@fun(color){
  brand: #6366f1;
  accent: #f59e0b;
  ink: #111827;
  paper: #f9fafb;
  muted: #6b7280;
}

@fun(font){
  body: 16px;
  small: 13px;
  h1: 2rem;
  h2: 1.5rem;
}

.page {
  background: @fun.color.paper.value;
  color: @fun.color.ink.value;
  font-size: @fun.font.body.value;
}

.page h1 { font-size: @fun.font.h1.value; color: @fun.color.brand.value; }
.page .tag { background: @fun.color.brand.value; color: #fff; padding: @fun.space.xs.value @fun.space.sm.value; }
```

Everything reads from one `@fun` block — tokens get a single source of truth.

## `@fun` vs `str()` vs `@define` — choosing

| Tool | Best for |
|---|---|
| `@fun` | Static key/value token stores, palette/spacing/type scales |
| `str()` | Reusable property-group fragments |
| `@define` | Parameterized logic/mixins (callable with arguments) |

## Gotchas

- Keys are accessed by the exact name you declare (`@fun.col.1.value` for a key `1`).
- `@fun.name.property.value` is the inline-value accessor — memorize this one.
- `@fun` blocks are data, not logic — don't put control flow inside them.

## Exercise

Create a `@fun(radius)` and a `@fun(shadow)` store, then style a `.modal` using values pulled from both via `.value` access.

## Key takeaways

- `@fun(name){ key: value; ... }`
- `@fun.name` → full block; `.property` → pair; `.property.value` → value.
- Ideal for design-token systems.

## Next

[Arrays with @arr](11-arr-arrays/README.md)