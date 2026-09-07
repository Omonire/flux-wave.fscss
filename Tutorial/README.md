# FSCSS Tutorials

Learn Figured Shorthand CSS (FSCSS) step by step. This curriculum takes you from your very first `.fscss` file to a complete, production-ready component built from FSCSS mixins.

FSCSS is a lightweight CSS preprocessor that compiles to plain CSS. It adds variables, reusable definition blocks, arrays, conditional event logic, semantic pattern matching, and powerful shorthand syntax on top of standard CSS — no runtime dependency in the compiled output.

## Prerequisites

- Basic knowledge of CSS (selectors, properties, `@media`, `@keyframes`).
- A code editor.
- Node.js (only if you want the CLI — browser runtime works without it).

## How to use this course

Every tutorial is a numbered folder with its own `README.md`. Each one contains:
- What you will learn
- Step-by-step FSCSS code
- The compiled CSS output
- Example HTML when relevant
- Gotchas and best practices

Work through the folders in order. The last tutorial is a capstone project that combines almost everything you learned.

## The 30 tutorials

| # | Folder | Topic |
|---|--------|-------|
| 01 | [`01-what-is-fscss`](01-what-is-fscss/README.md) | What is FSCSS? Philosophy, history, and when to use it |
| 02 | [`02-installation-setup`](02-installation-setup/README.md) | Every way to install and run FSCSS (NPM, CLI, CDN runtime, ESM) |
| 03 | [`03-first-fscss-file`](03-first-fscss-file/README.md) | Your first `.fscss` file and compile pipeline |
| 04 | [`04-variables-fundamentals`](04-variables-fundamentals/README.md) | Variables, the `!` force-eval marker, and design tokens |
| 05 | [`05-scoped-and-local-variables`](05-scoped-and-local-variables/README.md) | Local, scoped, and overridden variables |
| 06 | [`06-style-blocks-str`](06-style-blocks-str/README.md) | Reusable style fragments with `str()` |
| 07 | [`07-define-blocks`](07-define-blocks/README.md) | Reusable, parameterized blocks with `@define` |
| 08 | [`08-define-composition`](08-define-composition/README.md) | Composing components from multiple defines |
| 09 | [`09-block-defines-and-media`](09-block-defines-and-media/README.md) | Block defines, backtick strings, and `@media` generation |
| 10 | [`10-fun-stores`](10-fun-stores/README.md) | Design-token stores with `@fun` and dot access |
| 11 | [`11-arr-arrays`](11-arr-arrays/README.md) | Ordered collections with `@arr` |
| 12 | [`12-array-methods`](12-array-methods/README.md) | `.length`, `.first`, `.last`, `.join`, `.sum`, and friends |
| 13 | [`13-array-auto-indexing`](13-array-auto-indexing/README.md) | Generate rules with `@arr.name[]` auto-iteration |
| 14 | [`14-random`](14-random/README.md) | Compile-time and runtime randomness with `@random` |
| 15 | [`15-event-logic`](15-event-logic/README.md) | Conditional style logic with `@event` |
| 16 | [`16-event-comparisons-and-math`](16-event-comparisons-and-math/README.md) | Numeric comparisons and `num()` inside events |
| 17 | [`17-import-basics`](17-import-basics/README.md) | `@import` fundamentals: local, remote, and library imports |
| 18 | [`18-import-selective-aliases`](18-import-selective-aliases/README.md) | Selective imports and aliases |
| 19 | [`19-modular-architecture`](19-modular-architecture/README.md) | Build a scalable multi-file style system |
| 20 | [`20-shorthand-shared-values`](20-shorthand-shared-values/README.md) | `%2`–`%6` and `%i` shared-value shorthand |
| 21 | [`21-mx-and-mxs`](21-mx-and-mxs/README.md) | The `mx()` and `mxs()` multi-property helpers |
| 22 | [`22-attribute-selectors`](22-attribute-selectors/README.md) | `$(attribute:value)` attribute selector shorthand |
| 23 | [`23-keyframes-compact`](23-keyframes-compact/README.md) | Compact `@keyframes` blocks that bundle the animation property |
| 24 | [`24-vendor-prefixing`](24-vendor-prefixing/README.md) | Automatic vendor prefixes with `-*-` |
| 25 | [`25-num-calculations`](25-num-calculations/README.md) | In-stylesheet math with `num()` |
| 26 | [`26-count-and-length`](26-count-and-length/README.md) | Sequence and string-length utilities |
| 27 | [`27-copy-and-ext`](27-copy-and-ext/README.md) | Substring extraction with `copy()` and `@ext()` |
| 28 | [`28-rpt-and-pattern`](28-rpt-and-pattern/README.md) | Repetition with `rpt()` and semantic matching with `pattern()` |
| 29 | [`29-exec-debugging`](29-exec-debugging/README.md) | Console logging and pipeline control with `exec()` |
| 30 | [`30-capstone-component`](30-capstone-component/README.md) | Capstone: a complete animated gradient badge + a `flux-wave` remix |

## Official resources

- Documentation: https://fscss.devtem.org/docs
- NPM: https://www.npmjs.com/package/fscss
- GitHub: https://github.com/fscss-ttr/FSCSS
- Import guide: https://fscss.devtem.org/import
- Community: https://dev.to/fscss-ttr