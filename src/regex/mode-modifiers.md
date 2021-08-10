# Mode modifiers

Modes modifiers are specified outside the regular expression.

```
/pattern/modifiers;
```

* `i` - Make regex case insensitive, by **default** regex is case sensitive.

* `s` - for "single line mode" makes the dot match all characters, including line breaks. Not supported by Ruby or JavaScript.

* `m` - `^` and `$` match on every line.

* `g` - Perform a global match (find all matches rather than stopping after the first match). Regex by **default** stop immediately once match found and does not look another match, this modifier alter the behavior.

## Multiline mode

It only affects the behavior of `^` and `$`. In the multiline mode they match not only at the beginning and end of the string, but also at start/end of line.

**What do you mean by beginning and end of string?**

Say you provide file's content to regex engine then engine treats the whole file's content as one string which contains all characters including line break, space, tab etc. This is a **default** behavior of regex and if you want engine to treat each line in a file separated by line break as multiple stings and use `^` and `$` regex to be applied to each line then we use `m` modifier.

**regex:** `/^\d+/gm`

```js
let str = `1st place: Winnie
2nd place: Piglet
33rd place: Eeyore`;

//# With m
alert( str.match(/^\d+/gm) ); // 1, 2, 33

//# Without m
alert( str.match(/^\d+/g) ); // 1
```
