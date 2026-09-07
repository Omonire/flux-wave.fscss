# Tutorial 04 — Wave HTML Anatomy

The flux-wave markup is tiny but precise. Learn each node and why the mixins expect exactly this structure.

## The markup

```html
<div class="wave-container">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
</div>
```

## Node by node

### 1. The container — `wave-container`

The preset's first argument is **any selector** you like (`.wave-container` is the default). Everything from `flux-base`, `flux-container`, and `flux-bands` is scoped to it.

- `flux-base` resets margin/padding/box-sizing on `.wave-container` and everything inside it (the scoped reset).
- `flux-container` makes it `position: relative` + `overflow: hidden` + flex-centered, sized by `--flux-*` tokens.
- `overflow: hidden` is what *clips* the 140%-wide bands so they only peek through the rounded window.

```css
.wave-container, .wave-container * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.wave-container {
  position: relative;
  width: var(--flux-wrap-width, 100%);
  max-width: var(--flux-wrap-max-width, 560px);
  height: var(--flux-wrap-height, 160px);
  ...
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### 2. The shared class — `wave`

Every layer carries the `.wave` class. It is targeted by `flux-band` and gives all bands a shared shape:

```css
.wave-container .wave {
  position: absolute;
  width: 140%;
  left: -20%;
  border-radius: 50%;
  mix-blend-mode: screen;
  animation: flux-flow 4.5s ease-in-out infinite;
}
```

### 3. The numbered classes — `wave-1`, `wave-2`, `wave-3`, `wave-4`

These are produced by `flux-bands` from its internal `flux-i` array (`count(4,1)`). Each number pulls its own per-band tokens:

| Band | `--flux-bandN-bg` | `top` | `height` | `opacity` |
|---|---|---|---|---|
| 1 | cyan→green→purple→red | 35% | 80px | 0.75 |
| 2 | blue→purple→pink | 28% | 70px | 0.65 |
| 3 | green→indigo→red | 42% | 65px | 0.55 |
| 4 | sky→violet→orange | 38% | 55px | 0.45 |

Different offsets (top), sizes, opacities, **and delays** (`-1.1s`, `-2.3s`, `-0.7s`) make the bands desync so the wave looks organic instead of robotic.

## Why `wave-N` + token naming line up

Look at how the generated rule builds its property names:

```fscss
background: var(--flux-band@arr.@use(counter)[]-bg, none);
```

At loop iteration counter = 2 it reads the property `--flux-band2-bg`. So the **class suffix `-2` and the token suffix `2` must always exist together** in your data. If you add a `.wave-5` div but no `--flux-band5-bg`, it falls back to `none` (invisible). Remember: tokens and markup are two halves of the same table.

## Minimum viable markup

One band less than four? Sure — it just means you intentionally skip a layer:

```html
<div class="wave-container">
  <div class="wave wave-1"></div>
  <div class="wave wave-3"></div>   <!-- skipping wave-2 is allowed -->
</div>
```

The library doesn't care; it only generates CSS. But **semantic clarity** says: keep numbering sequential.

## Checkpoint

- You can name the container, the shared `wave` class, and the numbered variants.
- You know each band pulls its own `--flux-bandN-*` tokens.
- You know `overflow: hidden` on the container is what frames the wide bands.

Next — [05 · The flux-token system](05-the-flux-token-system/README.md)