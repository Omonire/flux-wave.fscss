# Tutorial 06 — Style Blocks with `str()`

`str(name, "css properties ...")` stores a reusable block of CSS text under a label. Writing the bare label inside a selector injects that block. It is the simplest way to reuse a fixed set of declarations without any parameters.

## Basic usage

```fscss
str(cardStyle, "
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  background: white;
  transition: transform 0.3s ease;
")

str(cardHover, "
  transform: translateY(-5px);
  box-shadow: 0 10px 15px rgba(0,0,0,0.1);
")

.product-card {
  cardStyle
  max-width: 300px;

  &:hover {
    cardHover
  }
}

.user-profile {
  cardStyle
  background: #f0f9ff;
}
```

Compiles to:

```css
.product-card {
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  background: white;
  transition: transform 0.3s ease;
  max-width: 300px;
}
.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 15px rgba(0,0,0,0.1);
}

.user-profile {
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  background: #f0f9ff;
  transition: transform 0.3s ease;
}
```

## How it works

1. `str(name, "...")` registers a named block of properties.
2. Inside any selector, the **bare label** (e.g. `cardStyle`) injects stored properties.
3. You can keep adding your own properties around the injected block.
4. A `str` can be referenced from multiple selectors — one definition, many uses.

## When to use `str()` vs `@define`

| | `str()` | `@define` |
|---|---|---|
| Parameters | None | Yes, with defaults |
| Call syntax | bare label | `@name(args)` |
| Use case | Fixed repeated groups (cards, resets, patterns) | Anything you need to parameterize (Tutorial 07) |

Since `str()` blocks are fixed, they are perfect for things like:

- Box-shadow standards
- Focus rings
- Truncation/ellipsis utilities
- Reset lines
- Common transition sets

## A utility example: text truncation

```fscss
str(truncate, "
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
")

truncate

.title { truncate width: 100%; }
.meta  { truncate width: 60%; }
```

`truncate` also injects a lone label without a selector — the utility applies globally to nothing by itself, and the two classes use it.

## Mixing with variables

`str` blocks can reference FSCSS variables from the surrounding scope:

```fscss
$radius: 8px;

str(panel, "
  background: #fff;
  border-radius: $radius!;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
")

.sidebar { panel padding: 1rem; }
```

## Gotchas

- The block content is plain CSS text; keep the quotes and newlines tidy (multi-line strings are fine).
- `str()` is not parameterized — if two usages need different values, build two blocks or switch to `@define`.
- Keep names consistent and descriptive; they behave like mixin names.

## Exercise

Define a `focusRing` style block, apply it to `a:focus`, `button:focus-visible`, and `.input:focus`, then verify the compiled output repeats it three times.

## Key takeaways

- `str(name, "...")` = named reusable CSS fragment.
- Inject with the bare label inside any selector.
- Best for fixed, non-parameterized groups.

## Next

[Reusable blocks with @define](07-define-blocks/README.md)