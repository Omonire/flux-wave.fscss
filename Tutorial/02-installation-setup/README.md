# Tutorial 02 — Installation & Setup

Every way to get FSCSS running, from a browser CDN to a full CLI build pipeline. Pick the one that matches your workflow.

## Before you start

FSCSS v1.2.0 ships two browser entry points and an npm package:

| File | Purpose |
|---|---|
| `runtime.js` / `runtime.min.js` | Auto-scans the page and compiles FSCSS on load. Use for prototyping and demos. |
| `esm.js` / `esm.min.js` | ES module build for tooling; never auto-runs. Call the API yourself. |
| `index.js` | npm package entry point (used by the CLI). |

## Option 1 — CDN runtime (browser, zero install)

Add the runtime script and a `<link>` pointing at your FSCSS file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>FSCSS demo</title>

  <!-- Link your FSCSS file -->
  <link type="fscss" href="style.fscss">

  <!-- v1.2.0 runtime: auto-compiles everything on load -->
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
</head>
<body>
  <button class="btn">Hello FSCSS</button>
</body>
</html>
```

`style.fscss`:

```fscss
$primary: #2563eb;

.btn {
  background: $primary!;
  color: #fff;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
}
```

The runtime compiles it in the browser. No Node installed, no build step.

> **Production warning:** browser compilation is for development and demos. Ship compiled CSS for production.

## Option 2 — NPM + CLI (production builds)

Install as a project dependency or globally:

```bash
# local, for a project
npm install fscss@latest

# global, for CLI access anywhere
npm install -g fscss
```

Compile:

```bash
fscss input.fscss output.css
```

Example — turn a token-heavy file into production CSS:

```bash
fscss styles/main.fscss dist/main.css
```

Then link `dist/main.css` normally:

```html
<link rel="stylesheet" href="dist/main.css">
```

No runtime script needed on the page anymore.

## Option 3 — ES modules (manual control)

If you want to trigger compilation yourself (route changes, dynamic styles, error handling):

```html
<script type="module">
  import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.0/esm.min.js";
  await xfscss.reboot();   // processes all <link type="fscss"> and <style>
</script>
```

The ESM build never auto-runs — you call `reboot()` when you are ready.

## Compiling styled `<style>` blocks too

The runtime processes not only `<link type="fscss">` files but also inline `<style>` blocks. For a quick demo without a separate file:

```html
<style>
$accent: #f59e0b;
h1 { color: $accent!; }
</style>
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" defer></script>
```

## NPM package entry

The npm package entry point is unchanged from 1.1.x, so all existing method calls keep working. Install for tooling integrations (build scripts, bundlers):

```bash
npm install fscss
```

## Which option should I choose?

| Situation | Choice |
|---|---|
| Quick prototype / CodePen-style demo | CDN runtime (`runtime.min.js`) |
| Production site | NPM CLI: `fscss style.fscss style.css` |
| SPA with async page loads | ESM build + manual `reboot()` |
| Bundler tooling | npm package entry `fscss` |

## Checklist

- [ ] Your `.fscss` file has no syntax errors (start small).
- [ ] The `<link type="fscss">` path is relative to the HTML page.
- [ ] For production, compile and ship pure CSS.

## Next

[Your first FSCSS file](03-first-fscss-file/README.md)