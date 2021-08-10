# Javascript support

## No support

* No `\A` or `\Z` anchors to match the start or end of the string. Use a caret or dollar instead.

* Lookbehind is not supported at all. Lookahead is fully supported.

* No atomic grouping or possessive quantifiers.

* No Unicode support, except for matching single characters with `\uFFFF`.

* No named capturing groups. Use numbered capturing groups instead.

* No conditionals.
