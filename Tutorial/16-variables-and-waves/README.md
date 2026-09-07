# Tutorial 16 — FSCSS Variables & Waves

`flux-wave.fscss` ships with token-defaults only. For a real theme you'll want FSCSS **variables** (`$name: value`) to define once and feed into your `--flux-*` overrides.

## The pattern

```fscss
$brand: #ff2ecb;
$ink: #0a0a12;

@flux-tokens()

:root {
  --flux-wrap-bg: $ink!;
  --flux-band1-bg: linear-gradient(90deg, transparent 0%, $brand! 50%, transparent 100%);
}

@flux-wave-preset(.wave-container)
```

`$brand!` forces evaluation at that spot, so the literal color lands inside the gradient string.

## Variable → token → band chain

Think of the pipeline:

```text
FSCSS variable  ($brand: #ff2ecb)
   ↓ evaluate with !
custom property  (--flux-band1-bg: linear-gradient(... $brand! ...))
   ↓ read via var()
compiled band    (background: var(--flux-band1-bg, none))
```

Three layers, but only the variable needs editing to re-theme.

## Full themed example

```fscss
$deep: #03050a;
$glow-a: #00e5ff;
$glow-b: #7c3aed;
$glow-c: #ff3d9a;

@flux-tokens()

:root {
  --flux-wrap-bg: $deep!;
  --flux-wrap-radius: 20px;

  --flux-band1-bg: linear-gradient(90deg, transparent, $glow-a! 45%, transparent);
  --flux-band2-bg: linear-gradient(90deg, transparent, $glow-b! 40%, transparent);
  --flux-band3-bg: linear-gradient(90deg, transparent, $glow-c! 40%, transparent);

  --flux-band1-duration: 5s;
  --flux-band2-duration: 6s;
  --flux-band3-duration: 5.5s;
}

@flux-wave-preset(.wave-container)
```

Now change `$glow-a` to `#7dd3fc` and the wave instantly re-skips. That's the variable lever.

## Labelling the gradient with copy() trick

FSCSS string utilities can derive tokens. This is optional but shows variables + tokens compose:

```fscss
body {
  background: $glow-c! copy(4, cv);
}
:root {
  --flux-band3-bg: linear-gradient(90deg, transparent, #ff3d9a 40%, transparent);
  --swatch: #cv;             /* illustrative: --swatch holds the sliced hex */
}
```

(Full `copy()`/`@ext()` coverage is a general-FSCSS topic; flux-wave's own design rarely needs it — the `$var!` pattern is enough for 95% of theming.)

## Mixing local variables inside wave components

For custom presets (later tutorials) scope variables inside your own `@define`:

```fscss
@define themed-wave(st: .tw){`
  $wave-hue: #22d3ee;
  :root {
    --flux-band1-bg: linear-gradient(90deg, transparent, $wave-hue! 45%, transparent);
  }
  @flux-wave-preset(@use(st))
`}
```

## Checkpoint

- Define colors once with `$` and feed them into `:root` token overrides.
- `$name!` = force evaluation at use point.
- Variables sit on top of the token system; the wave remains library-untouched.

Next — [17 · @fun stores for waves](17-fun-stores-for-waves/README.md)