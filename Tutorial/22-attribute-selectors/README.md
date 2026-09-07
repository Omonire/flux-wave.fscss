# Tutorial 22 — Attribute Selector Shorthand

The `$(attribute:value)` compact form compiles to a standard CSS attribute selector, letting you target elements by their attributes without verbosely writing brackets.

## Basic usage

```fscss
$(type:submit) {
  background: green;
  color: white;
}
```

Compiles to:

```css
[type='submit'] {
  background: green;
  color: white;
}
```

## Any attribute, any value

The shorthand is generic — use it for `href`, `title`, `data-*`, `lang`, `aria-*`, etc.:

```fscss
$(data-state: open) {
  opacity: 1;
}

$(lang: en) {
  quotes: "“" "”";
}

$(aria-current: page) {
  font-weight: 700;
  color: #3b82f6;
}
```

Compiles to:

```css
[data-state='open'] { opacity: 1; }
[lang='en'] { quotes: "“" "”"; }
[aria-current='page'] { font-weight: 700; color: #3b82f6; }
```

## Combining with another selector

The shorthand still works in combination:

```fscss
a$(href^="mailto:") {
  color: #16a34a;
}
```

That's `a[href^="mailto:"]` — attribute operator insight: attribute *operators* (`^=`, `*=`, `$=`) are standard CSS; the FSCSS shortcut covers the plain `[attr="value"]` form.

## Practical: styling a state machine

```fscss
$(data-toast: show) {
  transform: translateY(0);
  opacity: 1;
}
$(data-toast: hidden) {
  transform: translateY(-20px);
  opacity: 0;
}
```

Then toggling the `data-toast` attribute in your app instantly re-skines the component.

## Gotchas

- Output always uses single quotes around the value: `[type='submit']`.
- Use it for *equality* matching; reach for regular attribute selectors for pattern operators.
- Write it at the start of a selector or after an element tag.

## Exercise

Style `input[type='text']` and `div[data-role='tooltip']` using only `$(...)` shorthand, then verify the compiled rules.

## Key takeaways

- `$(attr:value)` → `[attr='value']`.
- Great for `data-*` behaviors and styled form states.

## Next

[Compact keyframes](23-keyframes-compact/README.md)