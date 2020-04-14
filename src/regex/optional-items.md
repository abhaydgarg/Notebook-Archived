# Optional items

The question mark `?` makes the preceding token in the regular expression optional. `colou?r` matches both `colour` and `color` because character `u` is optional.

You can make several tokens optional by grouping them together using parentheses, and placing the question mark after the closing parenthesis. E.g.: `Nov(ember)?` matches `Nov` and `November`.
