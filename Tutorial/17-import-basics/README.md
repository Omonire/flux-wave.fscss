# Tutorial 17 — `@import` Fundamentals

`@import` lets one `.fscss` file pull in another — or pull selected pieces from a library. This powers FSCSS's entire modular architecture. The classic form `@import(exec(file.fscss))` runs a file just as if its contents were pasted.

## Classic form

```fscss
@import(exec(_variables.fscss))
@import(exec(_mixins.fscss))
@import(exec(_buttons.fscss))
```

Each file executes in order — variables defined in `_variables.fscss` are usable by later files and by the importing file.

## Why split files?

- Variables in `_variables.fscss`, mixins in `_mixins.fscss`, components in their own files.
- Teams can work in parallel on separate files.
- Modules can be reused across projects.
- Compile-time keeps everything merged into a single CSS output.

## Remote & library imports

FSCSS can import from a built-in library, a local path, a package name, or a full URL:

```fscss
/* From the flux-wave remote library (as in this repo's README) */
@import((*) from flux-wave)

/* Named module from a file path */
@import((module) from "lib-or-path")

/* Remote from CDN / absolute URL */
@import((module) from "https://cdn.example/file.fscss")
```

The `flux-wave.fscss` sample even imports itself into an HTML page using the raw GitHub URL:

```fscss
@import((*) from "https://raw.githubusercontent.com/.../flux-wave.fscss/main/flux-wave.fscss")
```

## The three import stages

Internally the import pipeline runs three stages in order:

1. `impSel` — pick/selective selection
2. `impFrom` — resolve the `from` target
3. `procImp` — full import execution

You can disable individual stages when debugging (see Tutorial 29).

## When `@import` runs

Imports execute at compile time. In runtime mode the same processing happens on page load. There is no request per import at runtime — everything is inlined by the engine.

## Warning: circular imports

```
File A imports file B
File B imports file A  →  infinite loop, broken build
```

Design a clear directory tree (e.g. `_tokens` → `_mixins` → `_components` → `main`) to avoid cycles.

## A minimal multi-file project

```
styles/
  _variables.fscss
  _mixins.fscss
  main.fscss
```

`main.fscss`:

```fscss
@import(exec(_variables.fscss))
@import(exec(_mixins.fscss))

body {
  background: $light!;
  color: $dark!;
}
```

Compile:

```bash
fscss styles/main.fscss dist/style.css
```

## Gotchas

- Order matters — declare/import variables before you use them.
- `@import(exec(...))` is the classic "run this file" form.
- Beware cycles; keep the import graph a DAG.

## Exercise

Split your Tutorial-04 token system into `_tokens.fscss`, `_buttons.fscss`, `main.fscss` and import all three.

## Key takeaways

- `@import(exec(file.fscss))` includes a file at compile time.
- `@import((*) from name-or-url)` pulls from libraries/remotes.
- Imports are the backbone of modular FSCSS.

## Next

[Selective imports & aliases](18-import-selective-aliases/README.md)