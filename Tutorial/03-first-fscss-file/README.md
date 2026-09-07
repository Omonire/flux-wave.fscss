# Tutorial 03 — Your First FSCSS File

Write a `.fscss` file, compile it, and understand what the preprocessor does with it. This is the smallest complete example: variables, plain CSS, and a compiled output.

## Step 1 — Create the file

Make `style.fscss` in a project folder:

```fscss
$primary-color: #2563eb;
$border-radius: 8px;

.btn {
  background: $primary-color;
  color: white;
  padding: 12px 24px;
  border-radius: $border-radius;
  transition: all 0.3s ease;
}
```

Notice two things:
1. `$primary-color: #2563eb;` — a variable declaration.
2. `background: $primary-color;` — the variable is referenced with the `$` name but **no** `!` here.

## Step 2 — Compile

The fastest demo is the browser runtime (from Tutorial 02):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <link type="fscss" href="style.fscss">
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
</head>
<body>
  <button class="btn">Press me</button>
</body>
</html>
```

Or via CLI:

```bash
fscss style.fscss style.css
```

## Step 3 — The compiled output

```css
.btn {
  background: #2563eb;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  transition: all 0.3s ease;
}
```

The variable references were inlined with their values. The output is plain, standard CSS.

## The `!` marker

FSCSS variables can be resolved in two ways. When used *without* `!`, the value is inlined or mapped to a custom property depending on context. When used *with* `!`, the `!` forces evaluation at the point of use.

Compare:

```fscss
$accent: #f59e0b;

.a { border-bottom: 3px solid $accent!; }   /* forced, evaluates right here */
.b { border-bottom: 3px solid $accent; }     /* resolved during compile */
```

Both give the same result in this simple case; the difference matters when variables are re-assigned or computed (you will see why in Tutorials 04 and 05).

## Anatomy of a `.fscss` file

| Piece | Example | Purpose |
|---|---|---|
| Variable | `$color: red;` | Stores reusable values |
| Standard CSS | `.btn { ... }` | Everything you know still works |
| Custom property output | `--x: ...` | FSCSS compiles some things into native CSS variables |

Standard CSS rules, `@media`, `@keyframes`, comments, and even `@import` for fonts all remain valid inside FSCSS.

## Exercise

1. Add a second class `.btn:hover` that brightens the button.
2. Change the radius variable to `4px` and recompile.
3. Confirm the compiled CSS never contains a `$` symbol.

## Key takeaways

- `.fscss` files are CSS with superpowers.
- The compiler inlines (or maps) variables into clean CSS.
- Compiling can happen in the browser (runtime) or ahead of time (CLI).

## Next

[Variables fundamentals](04-variables-fundamentals/README.md)