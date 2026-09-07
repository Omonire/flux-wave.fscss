# Tutorial 15 — Conditional Logic with `@event`

`@event` is FSCSS's conditional function. It takes parameters and returns different values based on `if`, `el-if`, and `el` blocks — bringing real decision-making into stylesheets without a build script.

## Basic usage

```fscss
@event theme(mode) {
  if mode: dark {
    return: #111111;
  }
  el {
    return: #ffffff;
  }
}

body {
  background: @event.theme(dark);
  color: @event.theme(light);
}
```

Compiles to:

```css
body {
  background: #111111;
  color: #ffffff;
}
```

The first call returns the dark branch; the second falls through to `el`.

## Multiple conditions

```fscss
@event size(type) {
  if type: small {
    return: 12px;
  }
  el-if type: medium {
    return: 16px;
  }
  el-if type: large {
    return: 20px;
  }
  el {
    return: 24px;
  }
}

h1 { font-size: @event.size(large); }
```

Compiles to:

```css
h1 {
  font-size: 20px;
}
```

## Syntax rules

- `@event name(param) { ... }` defines the event.
- Branches: `if param: value { return: x; }`, `el-if`, `el`.
- Calling: `@event.name(argument)`.
- Only one branch's `return:` survives per call.

## Combining with variables

```fscss
$dark-bg: #111111;
$light-bg: #ffffff;

@event themedBackground(mode) {
  if mode: dark {
    return: $dark-bg!;
  }
  el {
    return: $light-bg!;
  }
}

body {
  background: @event.themedBackground(dark);
}
```

## Practical: a three-state button

```fscss
@event btnSkin(state) {
  if state: primary { return: #3b82f6; }
  el-if state: danger  { return: #ef4444; }
  el-if state: success { return: #10b981; }
  el { return: #64748b; }
}

.btn {
  background: @event.btnSkin(primary);
}
.btn.danger { background: @event.btnSkin(danger); }
.btn.ok     { background: @event.btnSkin(success); }
```

Now the button's color is a named decision, not magic hexes scattered around.

## Best practices

- Give events clear names: `theme`, `size`, `spacing`, `device`, `statusColor`.
- Keep each event to one responsibility — don't branch on unrelated concepts in the same event.
- Provide an `el` fallback so unknown inputs degrade safely.

## Gotchas

- Branches match on the *parameter value* you pass at the call site.
- `else` is written `el`; else-if is `el-if`.
- Returned values are strings; use `num()` (Tutorial 25) for math on them.

## Exercise

Write `@event fontScale(level)` with `small/body/title/display` returning sizes, then apply all four to a typography sampler.

## Key takeaways

- `@event name(param) { if ... el-if ... el ... }`
- Call with `@event.name(arg)`.
- The foundation of theme and state-driven styling.

## Next

[Comparisons and math inside events](16-event-comparisons-and-math/README.md)