# Tutorial 07 — Reusable Blocks with `@define`

`@define` creates reusable, **parameterized** style definitions — the FSCSS equivalent of a mixin. Parameters accept default values and are accessed inside the body with `@use(parameter)`.

This is the core building block behind real FSCSS component libraries (like `flux-wave.fscss`).

## Syntax

```fscss
@define name(param: default) {
  property: @use(param);
}
```

- `name` — the mixin name, called as `@name(...)`.
- Parameters with optional default values.
- `@use(param)` pulls the value in at the call site.
- Direct references such as `background: param;` are **not** resolved — you must use `@use()`.

## Basic example

```fscss
@define card(bg: black, color: white, pad: 20px, bd-r: 10px) {
  background: @use(bg);
  border-radius: @use(bd-r);
  padding: @use(pad);
  color: @use(color);
}

.box {
  @card(#007999)
}
```

Compiles to:

```css
.box {
  background: #007999;
  border-radius: 10px;
  padding: 20px;
  color: white;
}
```

Only the first parameter was passed; `bd-r`, `pad`, and `color` used their defaults.

## Passing some, letting others default

```fscss
.panel {
  @card(white, #333)      /* bg and color given, pad/bd-r from defaults */
}

.danger-zone {
  @card(#dc2626, white, 24px, 4px)
}
```

## Required-looking parameters

If a parameter has no default value, the caller is expected to pass it:

```fscss
@define chip(label, bg: #e2e8f0) {
  background: @use(bg);
  &:after { content: "@use(label)"; }
}

.tag { @chip("sale") }
```

## The `@use()` selector trick

Parameters are not limited to *values* — they can be selectors. This is exactly what `flux-wave.fscss` does:

```fscss
@define themed-btn(st) {
  @use(st) {
    background: #3b82f6;
    color: #fff;
  }
}

@themed-btn(.submit)
```

Compiles to:

```css
.submit {
  background: #3b82f6;
  color: #fff;
}
```

And with a default selector:

```fscss
@define wave(st: .wave) {
  @use(st) {
    position: absolute;
    left: 0;
  }
}

@wave()            /* uses .wave */
@wave(.w-blob)     /* scopes to a different class */
```

## Gotchas

- Values inside the block **must** use `@use(param)`.
- Defaults are applied when the caller omits the argument.
- Parameters can be values or selectors — a huge source of power.

## Exercise

Create an `@define avatar(size: 40px, radius: 50%, bg: #cbd5e1)` and call it with three variants: a plain avatar, a rounded-square one, and a large one.

## Key takeaways

- `@define name(params) { ... @use(param) ... }`
- Called with `@name(args)`.
- Defaults + being able to pass selectors make defines the Swiss-army knife of FSCSS.

## Next

[Composing components from defines](08-define-composition/README.md)