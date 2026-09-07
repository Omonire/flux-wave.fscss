# Tutorial 19 — Modular Architecture, End to End

Everything so far (variables, `str`, `@define`, `@fun`, `@import`) comes together in a **modular style system**: small focused files, imported into a `main.fscss`, compiled to one clean CSS file.

## The project layout

```
styles/
  _tokens.fscss        /* color, spacing, typography */
  _mixins.fscss        /* str() blocks: flexCenter, cardStyle, respondTo */
  _buttons.fscss       /* button component */
  _cards.fscss         /* card component */
  main.fscss           /* imports everything, page-level rules */
```

`_` prefix marks "partial" files meant to be imported, never linked directly.

## 1 — `_tokens.fscss`

```fscss
$primary: #3b82f6;
$secondary: #7c3aed;
$accent: #06b6d4;
$dark: #0f172a;
$light: #f8fafc;

$spacing-sm: 0.5rem;
$spacing-md: 1rem;
$spacing-lg: 2rem;

$font-main: 'Inter', sans-serif;
$font-heading: 'Inter', sans-serif;
```

## 2 — `_mixins.fscss`

```fscss
str(flexCenter, "
  display: flex;
  justify-content: center;
  align-items: center;
")

str(cardStyle, "
  padding: $spacing-md!;
  border-radius: 8px;
  background: $light!;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
")

str(respondTo, "
  @media (min-width: $breakpoint!) {
    @content;
  }
")
```

## 3 — `_buttons.fscss`

```fscss
.button {
  padding: $spacing-sm! $spacing-md!;
  border-radius: 4px;
  font-weight: 600;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
}

.btn-primary { background: $primary!; color: white; }

.btn-secondary { background: $secondary!; color: white; }
```

## 4 — `main.fscss`

```fscss
@import(exec(_tokens.fscss))
@import(exec(_mixins.fscss))
@import(exec(_buttons.fscss))

body {
  background: $light!;
  color: $dark!;
  font-family: $font-main!;
  line-height: 1.6;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 $spacing-md!;
}

.card {
  cardStyle
  margin-bottom: $spacing-md!;

  .card-title {
    font-size: 1.5rem;
    color: $primary!;
    margin-bottom: $spacing-sm!;
  }
}

.centered {
  flexCenter
  height: 100vh;
}
```

## 5 — Link & compile

Development (browser runtime):

```html
<link type="fscss" href="styles/main.fscss">
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
```

Production (single static CSS file):

```bash
fscss styles/main.fscss dist/main.css
```

You can also mix selective imports when vendoring libraries:

```fscss
@import((
  ui-card as card
) from "my-vendor/ui.fscss")
```

## Rules for a clean architecture

1. **One concern per file** — tokens, mixins, components, layout.
2. **Import in dependency order** — tokens first, then mixins, then components.
3. **Never commit generated CSS** alongside source unless you intend to ship it directly.
4. **Keep the graph acyclic** (no circular imports) — structure it as a layering tree.
5. **Compile for production** — the browser should receive one bundled stylesheet.

## Migration mindset

When a stylesheet grows beyond ~300 lines, split it:

- Pull every `$` into `_tokens.fscss`.
- Convert repeated blocks into `str()`/`@define`.
- Move component families into `_<name>.fscss`.
- Re-import everything from `main.fscss`.

## Gotchas

- Import ordering bugs look like "undefined variable" — check the import list order.
- `str()` blocks that reference `$breakpoint` need the variable defined in scope by the time the `str` is *injected* (define the default in `_tokens.fscss`).
- Don't link partials directly in HTML; only `main`-level files.

## Exercise

Take your previous tutorial components and split them into `_tokens`, `_mixins`, `_components`, `main`, then compile a production CSS file.

## Key takeaways

- Modular FSCSS = tokens + mixins + components + main.
- One compile command yields one clean CSS file.
- Selective imports keep vendor libs tidy.

## Next

[Shared-value shorthand `%n()`](20-shorthand-shared-values/README.md)