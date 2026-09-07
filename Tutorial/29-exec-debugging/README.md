# Tutorial 29 — `exec()` Debugging & Pipeline Control

`exec()` is FSCSS's developer console. It prints messages at compile time without producing CSS, and its `exec.obj.block(...)` form lets you disable import-pipeline stages during debugging or special builds.

## Console logging

```fscss
exec(_log, 'compiling theme module')
exec(_warn, 'deprecated variable name')
```

Both produce console output only — nothing lands in the compiled stylesheet. Use `_log` for informational messages and `_warn` for deprecations or suspicious values.

### Inspecting function results

Because `exec()` reads arbitrary strings, you can inspect what a utility would produce:

```fscss
exec(_log, "count(5)")     /* -> 1, 2, 3, 4, 5 */
exec(_log, "count(10, 2)") /* -> 2, 4, 6, 8, 10 */
```

Great for verifying `count()`, `length()`, or array methods before wiring them in.

## Logging during development

Place temporary logs next to tricky blocks:

```fscss
@fun(base){
  pad: 16px;
  radius: 8px;
}

exec(_log, "@fun.base.pad.value")
```

Then remove the debug line for production compile. (Alternatively, gate with a variable — see below.)

## Pipeline control with `exec.obj.block`

The import pipeline runs stages: `impSel`, `impFrom`, `procImp`. You can disable any of them:

```fscss
/* Skip ALL import handling */
exec.obj.block(f import);

/* Skip only selective / from stages */
exec.obj.block(f import pick);
exec.obj.block(f import from);
```

All `exec.obj.block(...)` markers are stripped from final CSS.

### When you need this

- Debugging an import that misbehaves: disable stages one by one to isolate the culprit.
- Special builds where you want raw files (no selective plumbing), only classic imports.
- Bundler/pipeline experiments where the default stages get in the way.

## Gating logs with an env-style variable

A cleaner pattern: only emit logs when a flag is on.

```fscss
$showLogs: verbose;

.if-logged {
  @event.logState($showLogs)
}
```

(Tie into `@event` if you want conditional `return` values rather than `exec` — `exec` is the logging tool; events are the branching tool.)

## Debugging checklist

1. Is the file being imported? Add `exec(_log, 'loaded _buttons')`.
2. Is the array length right? `exec(_log, "@arr.colors!.length")`.
3. Is a mixin being applied? Log right before the `@define` call.
4. Is an import behaving oddly? Disable pipeline stages incrementally.
5. Verify the final CSS — debug lines must never appear in it.

## Gotchas

- `exec()` is compile-time console output — it never creates CSS.
- `exec.obj.block(...)` markers are stripped, so safe to leave in debug builds.
- Keep logging out of production layouts even though harmless (they still pollute console).

## Exercise

Instrument a `@arr` + auto-indexing block from Tutorial 13 with log lines for array length and a parsed item, then use `exec.obj.block(f import)` to run a minimal file without import stages.

## Key takeaways

- `exec(_log, ...)` / `exec(_warn, ...)` → console diagnostics.
- `exec.obj.block(...)` → disable import stages.
- All exec forms are stripped from output CSS.

## Next

[Capstone: a complete component](30-capstone-component/README.md)