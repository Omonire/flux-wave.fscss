# Tutorial 30 — Capstone: A Complete Component System

Everything converges here. We build a production-style **animated gradient badge** component system — pure FSCSS — and remix it into the `flux-wave` pattern used by this repo. The result is a single portable preset you can drop into any page.

## The goal

- One file `badge-wave.fscss`
- Token mixin writing design tokens + `@keyframes`
- A preset mixin that composes the whole component
- Array-generated variants
- Importable as a library (`@import((*) from badge-wave)`)

## Step 1 — Tokens via a block define

Just like `flux-wave.fscss` uses `@define flux-tokens(root:root){...}`:

```fscss
@define badge-tokens(root:root){`
  :@use(root){
    --badge-size: 96px;
    --badge-bg: #0b0b12;
    --badge-radius: 50%;
    --badge-ring: rgba(255,255,255,0.12);
    --badge-dur: 3s;

    --badge-c1: #00c2ff;
    --badge-c2: #a855f7;
    --badge-c3: #22c55e;
  }

  @keyframes badge-spin {
    0%   { transform: rotate(0deg) scale(1); }
    50%  { transform: rotate(180deg) scale(1.15); }
    100% { transform: rotate(360deg) scale(1); }
  }
`}
```

Call it with `@badge-tokens()`. The block define (Tutorial 09) emits `:root` tokens **and** a full `@keyframes` — one mixin, two outputs.

## Step 2 — Base & container defines

```fscss
@define badge-base(st: .badge){`
  @use(st){
    position: relative;
    width: var(--badge-size, 96px);
    height: var(--badge-size, 96px);
    border-radius: var(--badge-radius, 50%);
    background: var(--badge-bg, #0b0b12);
    display: grid;
    place-items: center;
    overflow: hidden;
  }
`}
```

## Step 3 — Array-generated band variants

Use `count()` + auto-indexing (Tutorial 13) to fan out three sheen layers:

```fscss
@define badge-bands(st: .badge){`
@arr band-i[count(3, 1)]
  @use(st)::before:nth-of-type(@arr.band-i[]) {
    $i: @arr.band-i[];
    background: linear-gradient(120deg,
      transparent 0%,
      var(--badge-c@arr.band-i[], var(--badge-c1)) 50%,
      transparent 100%);
    animation: badge-spin var(--badge-dur, 3s)
               linear infinite;
    animation-delay: num($i! * 0.25)s;
  }
`}
```

`@arr.band-i[]` iterates `1,2,3`, and `--badge-c@arr.band-i[]` resolves to `--badge-c1`, `--badge-c2`, `--badge-c3`. Three bands from one block.

## Step 4 — The preset (composition)

```fscss
@define badge-wave(st: .badge){`
  @badge-tokens()
  @badge-base(@use(st))
  @badge-bands(@use(st))
`}
```

Consumers call a *single* preset — identical architecture to `flux-wave-preset` in `flux-wave.fscss`.

## Step 5 — Use it

```fscss
@import((*) from badge-wave)

@badge-wave(.spinner-logo)
```

```html
<div class="spinner-logo"></div>
```

Compiled output contains: the `:root` tokens, the `@keyframes badge-spin`, the base `.spinner-logo`, and `.spinner-logo::before:nth-of-type(1..3)` with staggered delays.

## Remix into a flux-wave mini

Reusing the same ideas, a compressed gradient-banner:

```fscss
@define mini-wave(st: .w){`
  @use(st){
    position: relative;
    height: 120px;
    background: #000;
    border-radius: 16px;
    overflow: hidden;
  }
`}

@define mini-bands(st: .w){`
@arr idx[count(4, 1)]
  @use(st) .band-@arr.idx[] {
    $i: @arr.idx[];
    position: absolute;
    width: 140%;
    height: var(--h, 60px);
    top: num(20 + $i! * 12)%;
    inset-inline: -20%;
    border-radius: 50%;
    filter: blur(14px);
    mix-blend-mode: screen;
    opacity: 0.8;
    background: var(--b@arr.idx[], #3b82f6);
  }
`}
```

Four generated `<div class="band-1..4">` layers, each reading its own `--b1..--b4` token. Add the `flux-flow` keyframes from `flux-wave.fscss` and you've rebuilt the whole wave component from first principles.

## Full skill map — what each piece used

| Step | Tutorials applied |
|---|---|
| Tokens in block define | 09 (block defines), 07 (params) |
| Base/container defines | 07, 08 |
| Band generation | 11, 13 (arrays + auto-index), 25 (`num`) |
| Preset composition | 08 |
| Importable library | 17, 18 (`@import((*) from ...)`) |
| Debugging while building | 29 (`exec(_log, ...)`) |

## Beyond: shipping your own library

1. Give the file a `package.json` like `flux-wave.fscss` (`extension: "fscss"`, `fscss_version: ">=1.2.0"`).
2. Publish/import remote: `@import((*) from "https://raw.githubusercontent.com/you/proj/main/badge-wave.fscss")`.
3. Document public mixins in a table, as the `flux-wave` README does.

## Final checklist

- [ ] Single `@preset(...)` call drives the whole component.
- [ ] Tokens + keyframes from one block define.
- [ ] Variants generated from `count()` arrays.
- [ ] Importable from a URL.
- [ ] Compiled output is clean, dependency-free CSS.

## Next steps after this course

- Explore the official library: https://fscss.devtem.org/libraries
- Join fscss-ttr on GitHub and publish your own module.
- Read the full docs: https://fscss.devtem.org/docs

You've completed all 30 tutorials. Go build.