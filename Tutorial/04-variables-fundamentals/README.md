# Tutorial 04 — Variables Fundamentals

Variables are the foundation of every FSCSS project. They store colors, spacing, fonts, and any repeated value — and they power design-token systems.

## Declaring variables

Global variables are declared at the top level:

```fscss
$primary: #3b82f6;
$secondary: #8b5cf6;
$global-font: 'Inter', sans-serif;

.button {
  background: $primary!;
  border: 2px solid $secondary!;
  font-family: $global-font!;
}
```

Compiles to:

```css
.button {
  background: #3b82f6;
  border: 2px solid #8b5cf6;
  font-family: 'Inter', sans-serif;
}
```

## The `!` force-evaluation marker

`$name!` forces the variable to be evaluated **at the point of use**. This is the marker you will see in almost every real stylesheet because it guarantees the exact value is read where you write it.

Why does it exist? Because without `!`, FSCSS may treat a reference as "resolve-able later" — which is perfect for computed/overridable values (see Tutorial 05). With `!` you get immediate, predictable evaluation.

Rule of thumb: when you want the literal value now, write `$name!`.

## Variables compile to native custom properties

In many cases FSCSS variables compile down to native CSS `--custom-properties`. This is why themes and runtime overrides work. Consider:

```fscss
$brand: #0ea5e9;
```

FSCSS may emit the value both inline (at usage) and as a custom property where the architecture calls for it. You can mix both worlds freely — a native CSS variable and an FSCSS variable can coexist:

```fscss
:root {
  --space: 1rem;
}

.card {
  padding: var(--space) $brand!;
}
```

## Practical example: a tiny design-token system

```fscss
$color-brand: #06b6d4;
$color-ink: #0f172a;
$color-paper: #f8fafc;

$space-sm: 0.5rem;
$space-md: 1rem;
$space-lg: 2rem;

$font-sans: 'Inter', system-ui, sans-serif;
$radius: 10px;

body {
  background: $color-paper!;
  color: $color-ink!;
  font-family: $font-sans!;
}

.alert {
  background: $color-brand!;
  color: white;
  padding: $space-sm! $space-md!;
  border-radius: $radius!;
}
```

This one set of variables now drives the whole page. Change one `$color-brand` and the brand color updates everywhere.

## Naming conventions

- Use descriptive prefixes: `$color-`, `$space-`, `$font-`, `$radius-`, `$z-`.
- Group related variables together in one block/file.
- Keep units consistent in spacing scales (all `rem` or all `px`).

## Gotchas

- A variable must be declared before use in the same pass (or imported — Tutorial 17).
- `$name!` is the explicit "use it now" form; prefer it for clarity.
- Variable names collide if you redeclare a global — that may be intentional (theming) or a bug.

## Exercise

Define variables for your favorite 3-color palette plus a spacing scale, then style a navbar using only those variables.

## Key takeaways

- Variables: `$name: value;`
- Usage: `$name!` forces immediate evaluation.
- They compile to clean values / native custom properties.

## Next

[Scoped & local variables](05-scoped-and-local-variables/README.md)