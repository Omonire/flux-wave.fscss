# Tutorial 09 — Block Defines & Media Generation

`@define` has a second mode: **block defines**. Instead of emitting declarations inside a rule, they emit a full CSS structure — media queries, keyframes, or whole sections — using a string block enclosed in backticks.

This is another technique used heavily by `flux-wave.fscss` (its token mixin writes a complete `@keyframes` block).

## Syntax

```fscss
@define name(params) {
  `
  @media (max-width: @use(size)) {
    ...
  }
  `
}

@name(...)
```

The backticks wrap a full block. Everything inside can use `@use(param)`.

## Example: a responsive container

```fscss
@define container(maxw: 1200px) {
  `
  .container {
    max-width: @use(maxw);
    margin: 0 auto;
    padding: 0 1rem;
  }
  `
}

@container()
```

Compiles to:

```css
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}
```

## Media queries from one define

```fscss
@define stack-on-mobile(breakpoint: 768px) {
  `
  @media (max-width: @use(breakpoint)) {
    .row { flex-direction: column; }
  }
  `
}

@stack-on-mobile()
@stack-on-mobile(600px)
```

Which emits two `@media` blocks — highly reusable without a build script.

## Block defines for keyframes

`flux-wave.fscss` defines its animation tokens as a backtick block containing `@keyframes flux-flow`. You can do the same for your own animations:

```fscss
@define bounce-keyframes(name: bounce) {
  `
  @keyframes @use(name) {
    0%   { transform: translateY(0); }
    50%  { transform: translateY(-10px); }
    100% { transform: translateY(0); }
  }
  `
}

@bounce-keyframes()
@bounce-keyframes(sway)
```

## Block defines can also embed other defines

Because it is just text, a block define can invoke other defines before it closes:

```fscss
@define section(pad: 3rem) {
  `
  .section {
    padding: @use(pad);
  }
  @container(980px)
  `
}
```

## Gotchas

- Block defines render standalone blocks, not properties inside an existing rule.
- Keep the backtick block well-formed CSS (it is injected verbatim after substitution).
- Selectors inside block defines are **not scoped** to the caller — they are full rules.

## Exercise

Create `@define grid-ish(cols: 3, gap: 1rem, breakpoint: 700px)` that emits a `.grid` rule and a narrowing `@media` — then call it for a 2-column variant.

## Key takeaways

- Block defines = backticks = full CSS structures.
- Great for `@media`, `@keyframes`, and template sections.
- Combine with value defines to build page machinery.

## Next

[Design-token stores with @fun](10-fun-stores/README.md)