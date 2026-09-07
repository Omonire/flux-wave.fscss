# Tutorial 02 — Setting Up FSCSS for flux-wave

flux-wave is a `.fscss` library. Before you can see a single wave you need FSCSS itself running, because `.fscss` is a *preprocessor format* — your browser cannot parse it until the FSCSS engine compiles it to plain CSS.

## Requirement check

flux-wave needs **FSCSS >= 1.2.0** (see `package.json`: `"fscss_version": ">=1.2.0"`). Version 1.2.0 structured its browser files into `runtime.min.js` and `esm.min.js`.

## Path A — Browser runtime (fastest way to play with waves)

Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>flux-wave demo</title>

  <!-- Load flux-wave from this repo, then use it in your own FSCSS -->
  <link type="fscss" href="style.fscss">

  <!-- FSCSS v1.2.0 runtime: finds every FSCSS link/style and compiles it -->
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
</head>
<body>
  <div class="wave-container">
    <div class="wave wave-1"></div>
    <div class="wave wave-2"></div>
    <div class="wave wave-3"></div>
    <div class="wave wave-4"></div>
  </div>
</body>
</html>
```

And `style.fscss` (your local file) pulls flux-wave in:

```fscss
@import((*) from flux-wave)

@flux-tokens()
@flux-wave-preset(.wave-container)
```

That is the entire recipe. The runtime compiles your `style.fscss`, which pulls in the `flux-wave` library exports, which emit the tokens, container, and band styles.

> **Path A note:** `@import((*) from flux-wave)` imports the library by name. In Path B below you see the equivalent raw-URL form.

## Path B — Raw URL import (no library registry needed)

Instead of importing by name you can point straight at the file in this repository:

```fscss
@import((*) from "https://raw.githubusercontent.com/Omonire/flux-wave.fscss/main/flux-wave.fscss")
```

Great for pinning an exact version/branch, or importing flux-wave in a place that has no module library configured.

## Path C — CLI compile (production)

When you are ready to ship, compile to real CSS:

```bash
npm install -g fscss
fscss style.fscss style.css
```

Then link the compiled result:

```html
<link rel="stylesheet" href="style.css">
```

No runtime script on the page. This is the "pure CSS" promise of flux-wave made concrete.

## Verifying your setup

1. Open the Path A page in a browser.
2. You should see a dark rounded strip with 4 colored, blurred bands drifting.
3. Open DevTools → Elements → inspect `.wave-container` — you should see the FSCSS-generated declarations (`position: relative; width: var(--flux-wrap-width, 100%); ...`) plus the generated `.wave-1` … `.wave-4` rules.
4. If nothing appears, open the console: `exec()` log lines surface there (covered in Tutorial 20).

## Common setup mistakes

| Mistake | Fix |
|---|---|
| Linking `flux-wave.fscss` directly as `<link type="fscss">` | You need your own FSCSS *file* that runs `@flux-tokens()` + `@flux-wave-preset()`. The library defines mixins; it does not apply them. |
| Wrong CDN version | Use `fscss@1.2.0/runtime.min.js`. |
| Forgetting the `type="fscss"` attribute | Plain `<link rel="stylesheet">` won't be compiled. |
| Importing by name without a configured library | Use Path B (raw URL). |

## Checkpoint

- You can run flux-wave with the browser runtime in under a minute.
- You know the compile-to-CSS path for production.
- You know the difference between "library defines mixins" and "your file invokes them".

Next — [03 · Your first wave](03-your-first-wave/README.md)