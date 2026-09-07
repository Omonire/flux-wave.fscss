# Tutorial 22 — Write Your Band Mixins

Re-create `flux-band` and `flux-bands` yourself. By the end you'll *own* the array-driven band generation that makes flux-wave work.

## Step 1 — the shared shape (`flux-band` clone)

```fscss
@define glow-band(st:.blob){`
  @use(st){
    position: absolute;
    width: var(--glow-w, 130%);
    left: var(--glow-l, -15%);
    border-radius: 50%;
    mix-blend-mode: screen;
    animation: glow-pulse var(--glow-dur, 2.4s) ease-in-out infinite;
  }
`}
```

Everything numerical jumped into `--glow-*` custom properties — a band's shared body, customizable per site.

## Step 2 — the generated variants (`flux-bands` clone)

The heart is the array loop. Declare a drive array, then use empty brackets `[]` to iterate it while synthesizing token names:

```fscss
@define glow-bands(st:.blob, counter:glow-i){`
@arr glow-i[count(3,1)]
  @use(st).p-@arr.@use(counter)[]{
    background: var(--glow-c@arr.@use(counter)[], var(--glow-c1));
    top: var(--glow-t@arr.@use(counter)[], 30%);
    animation-delay: var(--glow-d@arr.@use(counter)[], 0s);
  }
`}
```

Walk through one iteration (counter = 2):

| Piece | Becomes |
|---|---|
| selector | `.blob.p-2` |
| `--glow-c@arr.@use(counter)[]` | `--glow-c2` |
| `--glow-t@arr.@use(counter)[]` | `--glow-t2` |
| `--glow-d@arr.@use(counter)[]` | `--glow-d2` |

Three `.p-1/.p-2/.p-3` rules generated from one block; token names built the same way flux-wave builds `--flux-bandN-*`.

## Step 3 — pair with a container

```fscss
@define glow-shell(st:.blob){`
  @use(st){
    position: relative;
    width: var(--glow-size, 120px);
    height: var(--glow-size, 120px);
    background: var(--glow-bg, #0b0b18);
    border-radius: var(--glow-radius, 50%);
    overflow: hidden;
  }
`}
```

## Step 4 — compose the trio

```fscss
@define glow-badge(st:.blob){`
  @glow-tokens()
  @glow-shell(@use(st))
  @glow-band(@use(st))
  @glow-bands(@use(st))
`}
```

Then use:

```fscss
@glow-badge(.badge-spin)
```

```html
<div class="badge-spin">
  <div class="p p-1"></div>
  <div class="p p-2"></div>
  <div class="p p-3"></div>
</div>
```

You have re-architected flux-wave under a new name, with your own tokens, keyframes, and array-driven bands.

## Array-loop checklist when writing bands

- [ ] Drive array declared with literal `count(n, 1)`.
- [ ] Empty brackets `[]` in the selector AND in every token lookup.
- [ ] Token suffix matches the class suffix (both come from the same counter).
- [ ] Fallbacks on every `var()`.

## Checkpoint

- `.blob.p-@arr.@use(counter)[]` iterates and synthesizes selector + token names.
- Shared shape mixin + generated variants + shell + tokens = full component.
- Compose them into a one-call preset in the next tutorial.

Next — [23 · The preset pattern](23-the-preset-pattern/README.md)