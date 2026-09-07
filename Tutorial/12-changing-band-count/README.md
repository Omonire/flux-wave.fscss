# Tutorial 12 — Changing the Band Count

`flux-bands` defaults to **four** bands via its internal `flux-i` array. You can generate 2, 6, or 8 bands by passing your own array built with a real `count()` call.

## The default chain

- `flux-wave-preset(st, bandCount: flux-i)` passes its `bandCount` argument down to `flux-bands`.
- `flux-bands(st, counter: flux-i)` uses `counter` as the array to iterate.
- The internal `flux-i = count(4,1)` → 4 selectors.

To get a different count, hand in your own array.

## Making 6 bands

Declare the array first (it must be a *literal* `count()` call so it really expands):

```fscss
@arr my-bands[count(6,1)]
@flux-wave-preset(.wave-container, my-bands)
```

That generates `.wave-1` … `.wave-6`.

## The catch — tokens for bands 5 & 6

`flux-bands` only *generates selector blocks*. The **tokens** are a separate data table. Defaults ship tokans for bands 1–4 only. Bands 5–6 will fall back:

- `background` → `none` (invisible!)
- `top` → `30%`
- `height` → shared `--flux-band-height`
- etc.

So for visible extra bands, add their tokens:

```fscss
@arr my-bands[count(6,1)]

:root {
  --flux-band5-bg: linear-gradient(90deg, transparent, #fb7185 40%, transparent);
  --flux-band5-top: 22%;
  --flux-band5-delay: -0.4s;
  --flux-band5-duration: 6.5s;

  --flux-band6-bg: linear-gradient(90deg, transparent, #f97316 45%, transparent);
  --flux-band6-top: 50%;
  --flux-band6-delay: -1.8s;
  --flux-band6-duration: 7.2s;
}

@flux-wave-preset(.wave-container, my-bands)
```

And the HTML:

```html
<div class="wave-container">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
  <div class="wave wave-5"></div>
  <div class="wave wave-6"></div>
</div>
```

## Going down to 2

```fscss
@arr duo[count(2,1)]
@flux-wave-preset(.mini, duo)
```

Generates only `.wave-1` and `.wave-2`. Band 3–4 rules simply aren't emitted (delightfully, nothing to clean up).

## Keeping the three lists in sync

Changing band count always touches **three places**:

| List | What |
|---|---|
| `@arr your[n]` | how many selectors get generated |
| `:root` tokens | `--flux-bandN-*` for each visible band |
| HTML | one `.wave wave-N` per band |

Forget the tokens → bands render invisible/generic. Forget the HTML → extra CSS, missing visuals.

## Checkpoint

- Pass your own array: `@flux-wave-preset(sel, myBands)`.
- Add `--flux-bandN-*` tokens for every band beyond 4.
- Keep array count + tokens + HTML in sync.

Next — [13 · Controlling wave size](13-controlling-wave-size/README.md)