# Tutorial 05 — The flux-token System

Every knob of a flux-wave is a custom property. This is the library's design contract: **you don't edit mixins to restyle; you override tokens.**

Tokens are written by `flux-tokens()` into `:root` (the default parameter `root:root`). Let's tour the full set from the source.

## The code that writes them

```fscss
@define flux-tokens(root:root){`
  :@use(root){
    /* Container */
    --flux-wrap-width: 100%;
    --flux-wrap-max-width: 560px;
    --flux-wrap-height: 160px;
    --flux-wrap-bg: #000;
    --flux-wrap-radius: 28px;
    ...
  }
`}
```

## Group 1 — Container `--flux-wrap-*`

| Token | Default | Role |
|---|---|---|
| `--flux-wrap-width` | `100%` | stretch width |
| `--flux-wrap-max-width` | `560px` | cap width |
| `--flux-wrap-height` | `160px` | wave height |
| `--flux-wrap-bg` | `#000` | backdrop (bands blend on it) |
| `--flux-wrap-radius` | `28px` | rounded edges |

## Group 2 — Shared band defaults `--flux-band-*`

Defaults every band inherits unless overridden per band:

| Token | Default | Role |
|---|---|---|
| `--flux-band-width` | `140%` | band wider than container |
| `--flux-band-height` | `80px` | default band thickness |
| `--flux-band-left` | `-20%` | how far it spills left |
| `--flux-band-radius` | `50%` | pill shape |
| `--flux-band-filter` | `blur(12px)` | softness |
| `--flux-band-blend` | `screen` | how bands merge |
| `--flux-band-opacity` | `0.75` | default transparency |
| `--flux-band-anim` | `flux-flow 4.5s ease-in-out infinite` | animation shorthand |

## Group 3 — Per-band tokens `--flux-bandN-*`

Four copies of the same five tokens (band 1 shown; the pattern repeats for 2–4):

| Token | Band 1 default | Purpose |
|---|---|---|
| `--flux-band1-bg` | `linear-gradient(90deg, transparent 0%, #00c2ff 20%, #30d158 40%, #af52de 60%, #ff375f 80%, transparent 100%)` | the gradient wash |
| `--flux-band1-top` | `35%` | vertical offset |
| `--flux-band1-height` | `80px` (via default) | custom thickness |
| `--flux-band1-opacity` | `0.75` | custom alpha |
| `--flux-band1-filter` | `blur(12px)` | custom blur |
| `--flux-band1-delay` | `0s` | animation start offset |
| `--flux-band1-duration` | `4.2s` | animation speed |

Band 2 also varies its tokens: `--flux-band2-height: 70px`, `--flux-band2-opacity: 0.65`, `--flux-band2-delay: -1.1s`, `--flux-band2-duration: 5.1s`. Band 3 and 4 differ further (see Tutorial 10).

## How mixins consume the tokens

Two consumption styles coexist:

1. **Direct default:** `width: var(--flux-wrap-width, 100%);` — container reads its token with a fallback.
2. **Chained fallback:** `height: var(--flux-band1-height, var(--flux-band-height, 80px));` — band 1 uses its own token, otherwise the shared band default, otherwise a hard default.

This three-level ladder means flux-wave stays resilient: missing per-band token → shared default → hard default.

## Why `:root` and "once per page"

`flux-tokens()` defaults to `:root` because custom properties inherit. One token write at the page root is visible to **every** wave container, however many exist. That's the story of Tutorial 15.

## Checkpoint

- You can list the three token groups and their roles.
- You know the design contract: override tokens, don't edit mixins.
- You can explain the chained-fallback ladder.

Next — [06 · flux-base & flux-container](06-flux-base-and-container/README.md)