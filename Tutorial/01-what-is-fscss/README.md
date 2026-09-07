# Tutorial 01 — What is FSCSS?

FSCSS (Figured Shorthand Cascading Style Sheet) is a lightweight, modern CSS preprocessor. It keeps the CSS you already know and adds a layer of expressive features that compile down to plain, dependency-free CSS:

- **Shorthand syntax** — write repeated values in far fewer characters.
- **Reusable definitions** — `@define`, `@fun`, `pattern()`, and `str()` let you define a style once and reuse it everywhere.
- **Arrays & loops** — `@arr` generates multiple rules from one collection.
- **Conditional logic** — `@event` returns different values based on parameters and comparisons.
- **Automation** — auto vendor-prefixing, compact `@keyframes`, and attribute selector shorthand.

## Why does FSCSS exist?

Plain CSS has no variables-before-everything, no reusable blocks, no loops, and no logic. Developers traditionally reach for heavy toolchains (Sass/PostCSS + config + builds) to get those features. FSCSS tried to deliver the same power with a fraction of the setup:

- No config file required.
- Runs in the browser via a single CDN script for prototyping (v1.2.0 runtime auto-compiles on page load).
- Compiles ahead of time with a one-line CLI (`fscss input.fscss output.css`) for production.

## Philosophy

1. **CSS first.** Everything you write is still CSS. FSCSS augments rather than replaces; regular CSS stays valid inside a `.fscss` file.
2. **Compile pure.** The output is plain CSS with no runtime dependency.
3. **Shorthand by design.** The `%n()` family, `mx()`, `rpt()`, and compact keyframes exist to remove verbosity.
4. **Data-driven CSS.** Arrays and `count()` let small data generate large stylesheets (staggered animations, theming).

## Where it fits

| Need | Use plain CSS | Use FSCSS |
|---|---|---|
| One-off small page | ✅ | optional |
| Design-token system, reusable components | | ✅ |
| Generated/staggered animations | painful | ✅ |
| Conditional theming without JS | | ✅ |
| Production build pipeline | | ✅ (compile to CSS) |

## History in one minute

- **2022** — Conceived by Ekuyik Sam as a robust testing framework; early development by Figsh focuses on `%2`–`%6`, `%i`, and `$name:` variables.
- **2023** — `copy()` introduces first public test release.
- **2025** — Published to npm as `fscss`; console logging (`exec`), extended `%n()`, and a browser extension.
- **2026** — v1.1.25 adds array method extensions, `@event`, and `pattern()`. v1.2.0 consolidates browser entry points into `runtime.js` and `esm.js`.

FSCSS is published under Figsh Development and maintained through the `fscss-ttr` GitHub organization.

## The mental model

Think of a `.fscss` file as an **input program**. The FSCSS engine evaluates it (variables, defines, arrays, events) and emits a **static CSS stylesheet**. Every feature in this course follows the same pattern:

> Describe the rules with superpowers → compile → ship clean CSS.

## Key takeaways

- FSCSS compiles to clean, dependency-free CSS.
- All syntax is additive — standard CSS is always valid inside FSCSS.
- The features build on each other: variables → defines → arrays → events → imports.

## Next

[Installation & setup](02-installation-setup/README.md)