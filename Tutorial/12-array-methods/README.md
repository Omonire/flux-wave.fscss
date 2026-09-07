# Tutorial 12 — Array Methods

Arrays have a rich method library accessed with the `!` method-access marker: `@arr.name!.method`. These methods transform the collection or pull stats from it — all returning strings.

## The method-access switch

```fscss
@arr(n[10, 20, 30, 40]);

.list-output  { width: @arr.n.list; }        /* 10, 20, 30, 40 */
.count-output { width: @arr.n!.length; }     /* 4 */
```

## Reference table

| Method | Description | Example → Result |
|---|---|---|
| `.length` | Number of items | `@arr.n!.length` → `3` |
| `.first` | First item | `@arr.n!.first` |
| `.last` | Last item | `@arr.n!.last` |
| `.list` | Comma-separated list of all items | `@arr.n!.list` |
| `.join(sep)` | Join items with a separator | `@arr.n!.join(+)` → `10+20+30` |
| `.reverse` | Items in reverse order | `@arr.n!.reverse` |
| `.shuffle` | Items in random order | `@arr.n!.shuffle` |
| `.sort` | Alphabetic or numeric sort | `@arr.n!.sort` |
| `.unique` | Remove duplicates | `@arr.n!.unique` |
| `.indices` | 1-based index list | `@arr.n!.indices` → `1,2,3` |
| `.randint` | One random item | `@arr.n!.randint` |
| `.segment` | Wrap each item in brackets | `@arr.n!.segment` |
| `.sum` | Sum of numeric items | `@arr.num!.sum` |
| `.min` | Minimum numeric value | `@arr.num!.min` |
| `.max` | Maximum numeric value | `@arr.num!.max` |
| `.unit(val)` | Append a unit to each item | `@arr.n!.unit(px)` |
| `.prefix(v)` | Prepend a prefix to each item | `@arr.b!.prefix(btn-)` |
| `.surround(b,a)` | Wrap each with before/after | `@arr.b!.surround([,])` |

## Working examples

**Join + unit — build a spacing scale string:**

```fscss
@arr(scale[0.5, 1, 1.5, 2]);

.space-2x { padding: @arr.scale!.unit(rem); }   /* 0.5rem, 1rem, 1.5rem, 2rem */
```

**Prefix — batch class-generation data:**

```fscss
@arr(kinds[primary, accent, ghost]);

.label { font-style: italic; }
```

(Keep in mind: `.prefix` is for string data; combine with Tutorial 13 to drive selectors.)

**Stats — sum/min/max for dynamic sizing:**

```fscss
@arr(data[12, 40, 27, 8]);

.total  { width: num(@arr.data!.sum)px; }   /* 87px */
.hot    { height: @arr.data!.max px; }       /* highest value */
```

## Sorting & unique

```fscss
@arr(messy[3, 1, 3, 2, 1, 5]);

.clean { max-width: @arr.messy!.unique!.sort; }  /* deduped + sorted */
```

> **Remember:** methods return plain strings and do not chain, so `.unique!.sort` is not guaranteed — prefer running `.sort` on already-deduped data or computing separately.

## Combining with `@random`

Methods and randomness team up naturally (full `@random` coverage in Tutorial 14):

```fscss
@arr(backgrounds[#3b82f6, #8b5cf6, #06b6d4]);

.dynamic-card {
  background: @random(@arr.backgrounds);
}
```

## Gotchas

- Use `!` to enter method mode: `@arr.name!.method`.
- Results are strings — wrap numeric results with `num()` before math.
- Methods don't chain; plan transformations step by step.

## Exercise

Declare `@arr(scores[3, 7, 3, 9, 7, 2])`, then emit a line that uses `.length`, `.max`, `.sum`, and `.unique` in one compiled rule.

## Key takeaways

- `.length` `.first` `.last` `.list` `.join` are the everyday ones.
- `.unit`, `.prefix`, `.surround` shape items for output.
- `.sum` `.min` `.max` bring basic stats into CSS.

## Next

[Array auto-indexing](13-array-auto-indexing/README.md)