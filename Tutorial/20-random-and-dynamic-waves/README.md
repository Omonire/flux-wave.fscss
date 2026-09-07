# Tutorial 20 — Random & Dynamic Waves (with exec debugging)

`@random` picks a value randomly at compile (or render) time; `exec()` logs things to your console. Together they make waves that change *by themselves* — and are debuggable.

## Random gradient selection

```fscss
@arr(glows[
  linear-gradient(90deg, transparent, #00c2ff 45%, transparent),
  linear-gradient(90deg, transparent, #a855f7 45%, transparent),
  linear-gradient(90deg, transparent, #ff3d9a 45%, transparent)
]);

:root {
  --flux-band1-bg: @random(@arr.glows);
}
```

Each compile picks one of the three gradients for band 1. In runtime mode, each page render can re-pick → a subtly different wave every visit.

## Random durations & delays

```fscss
:root {
  --flux-band2-duration: num(@random([4, 5, 6]) * 1)s;
  --flux-band2-delay: -1s;
}
```

`num(...)s` turns the pick into a seconds value.

## Debugging with exec()

Stick a log line near fragile sections and read FSCSS's console output:

```fscss
exec(_log, 'building wave theme')
exec(_log, "count(4, 1)")   /* see what the band array produces */
exec(_warn, 'band 5 uses default tokens!')
```

`_log` = info, `_warn` = warning — both console-only, both stripped from compiled CSS.

## Verifying your arrays & tokens

Jump straight to the source: log the length of your band array.

```fscss
@arr my-bands[count(6,1)]
exec(_log, "@arr.my-bands!.length")   /* should print 6 */
```

If it prints something else, your `count()` call (or option) is off.

## Deterministic check

After debugging, confirm the production output:

```bash
fscss style.fscss style.css
grep flux-band1 style.css
```

You should see the resolved (or fallback) `--flux-band1-*` line. Randomness resolved *once* at compile — deterministic per build.

## Best practices

- Use `@random` on **color/duration deltas**, not structural stuff like selector counts (keep those deterministic).
- Keep `exec(_log, ...)` for development; strip or tolerate them in prod (they're logs only).
- Compile for production so ships pure CSS.

## Checkpoint

- `@random(@arr.name)` picks a value; `@random([a,b,c])` picks inline.
- `exec(_log, msg)` / `exec(_warn, msg)` surface in your console.
- Randomness is compile-time in CLI builds, render-time in runtime mode.

Next — [21 · Design your own tokens](21-design-your-own-tokens/README.md)