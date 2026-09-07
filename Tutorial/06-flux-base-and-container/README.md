# Tutorial 06 — flux-base & flux-container

The preset's second and third mixins build the wave's outer shell. Understanding them tells you exactly why a wave "just works" inside any layout.

## flux-base — the scoped reset

```fscss
@define flux-base(st:.wave-container){`
  @use(st), @use(st) *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
`}
```

Called with the container selector, it compiles to a **scoped** reset:

```css
.wave-container, .wave-container * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

- **Scoped** = only waved content is reset; your page's global style stays alone.
- **`box-sizing: border-box`** means the 160px height includes any borders/padding you add later.
- It does not touch the page — no global cascade damage.

## flux-container — the outer shell

```fscss
@define flux-container(st:.wave-container){`
  @use(st){
    position: relative;
    width: var(--flux-wrap-width, 100%);
    max-width: var(--flux-wrap-max-width, 560px);
    height: var(--flux-wrap-height, 160px);
    background: var(--flux-wrap-bg, #000);
    border-radius: var(--flux-wrap-radius, 28px);
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }
`}
```

Compiles to the container block (shown in Tutorial 03). Walk every line:

| Declaration | Why it matters |
|---|---|
| `position: relative` | Anchors absolutely-positioned bands |
| `width` / `max-width` | Stretch-to-fit but cap at 560px |
| `height: 160px` | Fixed band playground (override via token) |
| `background: #000` | Dark backdrop the `screen`-blended bands glow on |
| `border-radius: 28px` | The signature rounded look |
| `overflow: hidden` | **Clips** the 140%-wide bands to the rounded window |
| `display: flex` + center | Future content (text, logo) sits dead-center |

## Why `overflow: hidden` is the star

The bands are *wider than the container* (`140%`) and off-center (`left:-20%`). Without `overflow: hidden` the wave would smear all over your page. With it, you only ever see the moving slice inside the rounded rectangle — that's what makes the layers appear to "flow."

## Tuning the shell

Customize the container purely through tokens:

```fscss
@flux-tokens()

:root {
  --flux-wrap-max-width: 100%;
  --flux-wrap-height: 220px;
  --flux-wrap-radius: 16px;
  --flux-wrap-bg: #0a0a12;
}

@flux-wave-preset(.wave-container)
```

Compiles to a full-width, taller, squarer, near-black wave. No mixin changes.

## The selector-first design

Both mixins take `st:.wave-container` as their first parameter. That single design decision is why you can apply a wave under `.wave-hero`, `.wave-footer`, or any class — the whole library keys off the `st` argument.

## Checkpoint

- `flux-base` = scoped reset (doesn't touch the page).
- `flux-container` = relative, overflow-hidden, flex-centered frame.
- Override the shell with `--flux-wrap-*` tokens only.

Next — [07 · flux-band & flux-bands](07-flux-band-and-bands/README.md)