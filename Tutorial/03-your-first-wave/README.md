# Tutorial 03 — Your First Wave

You have FSCSS running. Now put **your first live flux-wave** on the page — three lines of FSCSS and four empty `<div>`s.

## The complete recipe

`style.fscss`:

```fscss
@flux-tokens()
@flux-wave-preset(.wave-container)
```

`index.html`:

```html
<div class="wave-container">
  <div class="wave wave-1"></div>
  <div class="wave wave-2"></div>
  <div class="wave wave-3"></div>
  <div class="wave wave-4"></div>
</div>
```

That's it. Refresh, and a dark rounded strip fills with four blurred gradient bands that flow and shift forever.

## What each of those two lines does

| Line | What it expands to |
|---|---|
| `@flux-tokens()` | Writes all `--flux-*` custom properties to `:root`, and emits `@keyframes flux-flow` |
| `@flux-wave-preset(.wave-container)` | Runs the other five mixins scoped to `.wave-container` |
| `.wave-container` argument | The **selector** — your container gets position relative, sizing, overflow hidden, centering |
| children `.wave wave-N` | Styled by `flux-band` + `flux-bands` as absolute blurred gradient layers |

## What you should see at each detail level

Open DevTools and look at the compiled rules:

**The container** (from `flux-container`):

```css
.wave-container {
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
```

**One band** (from `flux-band`):

```css
.wave-container .wave {
  position: absolute;
  width: var(--flux-band-width, 140%);
  left: var(--flux-band-left, -20%);
  border-radius: var(--flux-band-radius, 50%);
  mix-blend-mode: var(--flux-band-blend, screen);
  animation: var(--flux-band-anim, flux-flow 4.5s ease-in-out infinite);
}
```

**Band 1** (from `flux-bands`, generated):

```css
.wave-container .wave-1 {
  background: var(--flux-band1-bg, none);
  top: var(--flux-band1-top, 30%);
  height: var(--flux-band1-height, var(--flux-band-height, 80px));
  opacity: var(--flux-band1-opacity, var(--flux-band-opacity, 0.75));
  filter: var(--flux-band1-filter, var(--flux-band-filter, blur(12px)));
  animation-delay: var(--flux-band1-delay, 0s);
  animation-duration: var(--flux-band1-duration, 4.5s);
}
```

Notice the **fallback pattern**: every custom property has a default (`var(--x, default)`), so even if a token is missing, the wave still renders. This is deliberate library design — waves never break silently.

## The band-count mismatch gotcha

The preset's `flux-bands` default generates **four** band selectors (`.wave-1`…`.wave-4`). Your HTML should match. If you write 3 `<div>`s you'll still compile fine but have an unused `.wave-4` rule; if you write 5, the fifth gets nothing. Keeping HTML in sync is your job — Tutorial 12 covers changing the count on both sides.

## Checkpoint

- `@flux-tokens()` + `@flux-wave-preset(sel)` produces a full wave.
- Every token has a fallback default.
- Band count in HTML should match the generated selectors.

Next — [04 · Wave HTML anatomy](04-wave-html-anatomy/README.md)