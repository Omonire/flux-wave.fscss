# Tutorial 18 — Selective Imports & Aliases

The modern selective form (v1.1.16+/v1.2.0) imports only the pieces you need — and optionally renames them. This keeps namespaces clean, avoids collisions, and trims what actually gets merged into your build.

## Why selective imports?

When a module library exports many `@define` blocks, mixins, or `str` blocks, importing everything can create name clashes. Selective imports let you:

- pick exactly which exports you need,
- give them short, local aliases,
- leave the rest of the module out of your stylesheet.

## Selective import with aliases

```fscss
@import((
  circle-progress as clp,
  progress-range as pr,
  progress-root as p-root
) from circle-progress)

/* Use the aliases */
@p-root()
@clp(.progress-circle)

.p72 {
  @pr(72)
}
```

This is the same pattern used by the official `fscss-modules` library.

## Selective import without renaming

```fscss
@import((
  primaryBtn,
  secondaryBtn
) from "my-buttons.fscss")

.cta { @primaryBtn }
.alt { @secondaryBtn }
```

## The classic import is still fine

```fscss
@import(exec(_buttons.fscss))
```

Use the classic form for small local files where you want *everything*; use the selective form when importing from a library or when you only need specific exports.

## The pipeline mapping

Selective imports map onto the import stages:

| What you write | Stage used |
|---|---|
| `@import((mod) from X)` | `impSel` (pick) + `impFrom` (from) |
| `@import(exec(file))` | `procImp` (full import) |

You can override these per build with `exec.obj.block(...)` (Tutorial 29).

## Example: avoiding collisions

Two libraries both export `card`:

```fscss
@import((
  card as uiCard,
  card-header as hdr,
  card-body as bdy
) from "ui-kit.fscss")

@import((
  card as ecoCard
) from "eco-theme.fscss")

.dashboard-panel { @uiCard }
.store-card      { @ecoCard }
```

Same name, zero conflicts, both themes coexist.

## Best practices

- Prefer aliases that document intent: `flexCenter as center`.
- Only import what you use — smaller compiled output, fewer conflicts.
- Keep aliases consistent across the project (document them in a header comment).

## Gotchas

- Aliased names only exist after the import line — use them after it.
- Don't alias away names you still reference elsewhere in plain CSS.
- Ordering still matters; imports resolve top to bottom.

## Exercise

Import just `cardStyle` and `flexCenter` (aliased from `_mixins.fscss`) into a page and style a hero + panel with only those two.

## Key takeaways

- `@import((a as x, b as y) from source)` picks and renames.
- Selective imports avoid collisions and trim output.
- Aliases + `from` map to the `impSel`/`impFrom` stages.

## Next

[Modular architecture, end to end](19-modular-architecture/README.md)