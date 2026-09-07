# Tutorial 01 — Introducing flux-wave

flux-wave is a `.fscss` component: flowing, layered gradient wave bands built entirely from FSCSS mixins. It produces **pure CSS** — no JavaScript, no runtime library — and it is this repository's reason to exist.

## What you are looking at

```text
flux-wave.fscss   <- the FSCSS source (what this whole repo is about)
package.json      <- FSCSS package metadata (version, extension, deps)
README.md         <- the library readme
Tutorial/         <- 30 lessons, starting here
```

Open `flux-wave.fscss` now and glance at it. You do not have to understand everything — but notice the shape. The file is a list of `@define` mixins:

```fscss
@define flux-tokens(root:root){...}      /* design tokens + keyframes */
@define flux-base(st:.wave-container){...}      /* scoped reset */
@define flux-container(st:.wave-container){...} /* outer shell */
@define flux-band(st:.wave){...}                /* shared band shape */
@define flux-bands(st:.wave, counter:flux-i){...} /* generated band variants */
@define flux-wave-preset(st:.wave-container, bandCount:flux-i){...} /* one-call composite */
```

That is the architecture of the whole library: **six public mixins, one preset** that wires them together.

## The design idea

A flux-wave is a dark, rounded strip (the container) containing several wide, blurred, semi-transparent gradient ellipses (the bands). Each band:

- is wider than the container and slightly off-center (`width:140%`, `left:-20%`),
- has a rounded pill shape (`border-radius:50%`) and heavy blur (`blur(12px)`),
- blends with its siblings using `mix-blend-mode: screen`,
- animates with a shared `flux-flow` keyframe that translates, scales vertically, and skews over time.

Shifting each band's **height**, **opacity**, **delay**, and **duration** makes the layers swim past each other — the "flux" illusion.

## The four principles

1. **Tokens, not hard-coded values.** Every size, gradient, blur, and timing is a `--flux-*` custom property so you can re-skin without editing mixins.
2. **One token call per page.** `flux-tokens()` writes its values to `:root` once; multiple waves share them.
3. **Arrays generate repetition.** Four bands come from a `count(4,1)` loop, not four identical blocks.
4. **Preset over plumbing.** Consumers call `@flux-wave-preset(...)`; nobody has to know the six pieces.

## What you will need (checked in Tutorial 02)

flux-wave requires **FSCSS >= 1.2.0**. You can run it:

- in the browser with the `runtime.min.js` CDN script, or
- compiled with the CLI: `fscss yourfile.fscss output.css`.

## Key terminology you will keep using

| Term | Meaning |
|---|---|
| wave-container | The outer rounded shell (`.wave-container`) |
| band | A single gradient layer (`.wave-1`, `.wave-2`, …) |
| tokens | The `--flux-*` custom properties in `:root` |
| preset | The `flux-wave-preset` one-call composite |

## Checkpoint

- You can name the six public mixins.
- You can explain why bands look like blurred moving ellipses.
- You know flux-wave outputs pure CSS and needs FSCSS >= 1.2.0.

Next — [02 · Setting up FSCSS](02-setting-up-fscss/README.md)