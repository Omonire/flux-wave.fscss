# flux-wave Training — 30 Tutorials

A complete, hands-on course for **flux-wave.fscss** — the flowing, layered gradient wave library built entirely from FSCSS mixins.

Every tutorial is grown around the real `flux-wave.fscss` source in this repository. You will go from "I have never opened a `.fscss` file" to "I can revamp, extend, and ship my own flux-wave components." Along the way you learn exactly the FSCSS skills the library is built from: `@define` mixins, design tokens, array-generated bands, compact keyframes, and the preset composition pattern.

## What flux-wave.fscss is

- **Pure CSS.** No JavaScript. The compiled output is plain CSS.
- **FSCSS mixins.** Built from `@define` blocks: `flux-tokens`, `flux-base`, `flux-container`, `flux-band`, `flux-bands`, and a one-call `flux-wave-preset`.
- **Design tokens.** All sizing, gradients, timing, and blur live in `--flux-*` custom properties you can override.
- **Array-generated.** Four gradient bands are produced from a `count(4,1)` array loop — not four hand-written blocks.

## How the course is organized

| Part | Folders | What you master |
|---|---|---|
| 1 — Foundations | 01–05 | What flux-wave is, FSCSS setup, first wave, HTML anatomy, the token system |
| 2 — The Machine Room | 06–10 | Container/base mixins, band generation, the `flux-flow` animation, per-band styling |
| 3 — Customization | 11–15 | Brand overrides, band counts, sizes, timing, multiple waves per page |
| 4 — FSCSS Power-Ups | 16–20 | Variables, `@fun` stores, `@event` themes, modular imports, `@random` |
| 5 — Build Wave Components | 21–30 | Build your own tokens, mixins, presets, loaders, badges, publish, and a final capstone |

## The 30 tutorials

| # | Folder | Lesson |
|---|--------|--------|
| 01 | [`01-introducing-flux-wave`](01-introducing-flux-wave/README.md) | What flux-wave is, what it ships, and the ideas behind it |
| 02 | [`02-setting-up-fscss`](02-setting-up-fscss/README.md) | Set up FSCSS v1.2.0 so flux-wave can run (runtime, CLI, props) |
| 03 | [`03-your-first-wave`](03-your-first-wave/README.md) | Put a live wave on a page in 3 lines of FSCSS |
| 04 | [`04-wave-html-anatomy`](04-wave-html-anatomy/README.md) | The `wave-container` + `wave-N` structure element by element |
| 05 | [`05-the-flux-token-system`](05-the-flux-token-system/README.md) | Every `--flux-*` token: container, band, and per-band defaults |
| 06 | [`06-flux-base-and-container`](06-flux-base-and-container/README.md) | The scoped reset and outer shell mixins |
| 07 | [`07-flux-band-and-bands`](07-flux-band-and-bands/README.md) | Shared band shape and the generated band variants |
| 08 | [`08-the-flux-flow-animation`](08-flux-flow-animation/README.md) | The `flux-flow` keyframes and its transform math |
| 09 | [`09-array-generated-bands`](09-array-generated-bands/README.md) | How `count(4,1)` + auto-indexing produces `wave-1..4` |
| 10 | [`10-per-band-styling`](10-per-band-styling/README.md) | Position, height, opacity, blur, delay, duration per band |
| 11 | [`11-overriding-tokens`](11-overriding-tokens/README.md) | Re-skin waves with `:root` overrides, no mixin edits |
| 12 | [`12-changing-band-count`](12-changing-band-count/README.md) | 2, 6, or 8 bands via a custom `count()` array |
| 13 | [`13-controlling-wave-size`](13-controlling-wave-size/README.md) | Width, max-width, height, radius, responsive scales |
| 14 | [`14-timing-and-motion`](14-timing-and-motion/README.md) | Delays, durations, easing, negative-delay trick, infinite loops |
| 15 | [`15-multiple-waves-per-page`](15-multiple-waves-per-page/README.md) | Heroes, footers, and many waves from one token call |
| 16 | [`16-variables-and-waves`](16-variables-and-waves/README.md) | FSCSS `$` variables feeding `--flux-*` tokens |
| 17 | [`17-fun-stores-for-waves`](17-fun-stores-for-waves/README.md) | `@fun` palettes and spacing stores powering waves |
| 18 | [`18-event-powered-themes`](18-event-powered-themes/README.md) | `@event`-driven dark/light/brand wave themes |
| 19 | [`19-modular-wave-projects`](19-modular-wave-projects/README.md) | `@import` + file structure for larger wave sites |
| 20 | [`20-random-and-dynamic-waves`](20-random-and-dynamic-waves/README.md) | `@random` band colors and `exec()` debugging |
| 21 | [`21-design-your-own-tokens`](21-design-your-own-tokens/README.md) | Rebuild `flux-tokens` with a block `@define` |
| 22 | [`22-write-your-band-mixins`](22-write-your-band-mixins/README.md) | Rebuild `flux-band` + `flux-bands` yourself |
| 23 | [`23-the-preset-pattern`](23-the-preset-pattern/README.md) | Compose `flux-wave-preset` and your own presets |
| 24 | [`24-wave-loaders`](24-wave-loaders/README.md) | Loaders, spinners, and boot screens from wave technique |
| 25 | [`25-gradient-badge-remix`](25-gradient-badge-remix/README.md) | A wave-style badge remixing tokens + arrays |
| 26 | [`26-shorthand-and-helpers`](26-shorthand-and-helpers/README.md) | `%n`, `mxs`, vendor prefixes inside wave helpers |
| 27 | [`27-responsive-and-accessible`](27-responsive-and-accessible/README.md) | Media queries and `prefers-reduced-motion` for waves |
| 28 | [`28-publishing-your-wave`](28-publishing-your-wave/README.md) | `package.json` meta, health, GitHub README, remote import |
| 29 | [`29-flux-ecosystem`](29-flux-ecosystem/README.md) | The FSCSS module library and where flux-wave fits |
| 30 | [`30-capstone-flux-landing`](30-capstone-flux-landing/README.md) | Ship a themed landing page with waves, loaders, and a badge |

## Prerequisites

- Basic HTML/CSS comfort.
- Nothing else — FSCSS gets installed in Tutorial 02.

## Official FSCSS references used along the way

- Docs: https://fscss.devtem.org/docs
- Import guide: https://fscss.devtem.org/import
- NPM: https://www.npmjs.com/package/fscss
- GitHub: https://github.com/fscss-ttr/FSCSS

Start with [01 — Introducing flux-wave](01-introducing-flux-wave/README.md).