# Tutorial 05 — Scoped & Local Variables

Variables are not only global. FSCSS variables declared inside a selector are scoped to that block, which gives you locality, override chains, and theme switching without fighting the cascade.

## Local variables

A variable declared inside a ruleset is scoped to that ruleset:

```fscss
.card {
  $local-bg: #f1f5f9;
  background: $local-bg!;
  padding: 1rem;
}

.card-alt {
  /* $local-bg is NOT visible here — it belongs to .card */
  background: white;
}
```

## Scope chain & overrides

Because globals exist, you can create a chain: global default → selectors that override for a specific context. Example — a themable button:

```fscss
$radius: 6px;
$btn-bg: #3b82f6;

.danger {
  $btn-bg: #ef4444;   /* local override visible in this block */
}

.btn {
  background: $btn-bg!;
  border-radius: $radius!;
}

.danger .btn {
  background: $btn-bg!;   /* now #ef4444 because we are inside .danger */
}
```

## Theme switching with global re-declaration

You can redeclare globals to re-skin an entire page. Combined with `@event` (Tutorial 15) or a parent class, this turns into a full theming system:

```fscss
$bg: #ffffff;
$fg: #111111;

body.dark {
  $bg: #0f172a;
  $fg: #f8fafc;
}

body {
  background: $bg!;
  color: $fg!;
}
```

At compile time FSCSS resolves `$bg!` inside `body.dark` to the dark values and inside `body` to the light values, producing two rules that ship clean CSS.

## Using `!` with re-assignment

This is where the `!` marker earns its keep. Without `!`, resolution can be deferred; with `!` you pull the value where the scale expects it. Keep overrides next to usage to make the intent obvious:

```fscss
$spacing: 1rem;

.featured {
  $spacing: 2rem;
  padding: $spacing!;
}
```

## Practical pattern: component-scoped tokens

A common FSCSS pattern is declaring the *component's* tokens at the top of its block so the whole group reads from one place:

```fscss
.accordion {
  $item-bg: #f8fafc;
  $item-hover: #e2e8f0;
  $chevron: #64748b;

  .accordion-item { background: $item-bg!; }
  .accordion-item:hover { background: $item-hover!; }
  .accordion-chevron { color: $chevron!; }
}
```

## Gotchas

- Local variables do not leak out of the block they are declared in.
- Redeclaring a global inside a selector creates a scoped shadow for that block only.
- Keep declarations before uses **inside the same block** to avoid ordering surprises.

## Exercise

Build a `.callout` component with three modifier classes (`.info`, `.warn`, `.error`) by declaring a scoped `$tint` in each modifier and using it in the shared callout styles.

## Key takeaways

- Local variables are scoped to their block.
- Contexts can override globals locally.
- This is the foundation for themes and component styling.

## Next

[Style blocks with str()](06-style-blocks-str/README.md)