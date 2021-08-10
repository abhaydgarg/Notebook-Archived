# Backreferences

## Backreferences match the same text as previously matched by a capturing group

Suppose you want to match a pair of opening and closing HTML tags. By putting the opening tag into a backreference, we can reuse the name of the tag for the closing tag. Here's how: `<([A-Z][A-Z0-9]*)\b[^>]*>.*?</\1>`. This regex contains only one pair of parentheses, which capture the string matched by `[A-Z][A-Z0-9]*`. This is the opening HTML tag. The backreference `\1`(backslash one) references the first capturing group which is used for closing tag.

## Backreference position

Scan the regular expression from **left to right** for parentheses and the first parenthesis starts backreference number one, the second number two, etc.

> Skip parentheses that are part of other syntax such as non-capturing groups and inside character classes `[]`.

## Parenthesis inside character class []

When you put a parenthesis in a character class, it is treated as a literal character. So the regex `[(a)b]` matches a, b, (, and ). `(a)` inside `[]` does not contribute to backreference.
