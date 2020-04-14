# Grouping

A part of the pattern can be enclosed in parentheses `()`. That’s called a “capturing group”.

That has two effects:

1. It allows to place a part of the match into a separate array.
2. If we put a quantifier (such as `+`, `*`, `{min,max}, ?`) after the parentheses, it applies to the parentheses as a whole, not the last character.

For example, regex `(go)+` finds one or more `go`. Without parentheses, the pattern `go+` means `g`, followed by `o` repeated one or more times.

## Capturing

```js
let str = '<h1>Hello, world!</h1>';

let reg = /<(.*?)>/g;

let match;

while (match = reg.exec(str)) {
  // first shows the match: `<h1>` is a whole match, and `h1` is group capturing inside ()
  // then shows the match: `</h1>` is a whole match, and `/h1` is group capturing inside ()
  alert(match);
}
```

## Non capturing

Use `?:` to avoid capturing group enclosed within `()`.

For example, in regex above `<(.*?)>`, if you do not want to capture group in separate array then rewrite the regex as `<(?:.*?)>`.
