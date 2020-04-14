# Special character

## Literal characters

Character has no special meaning. Match `a` in a string `Jack is a boy`. It matches the `a` after the `J`. The `a` is in the middle of the word and it does not matter to the regex engine. If it matters to you, you will need to tell that to the regex engine by using `word boundaries`. The regex can match the second `a` too. It only does so when you tell the regex engine to start searching through the string after the first match.

## Special characters

> **Metacharacters**

There are **12** characters with special meanings:

1. backslash `\`
2. caret `^`
3. dollar sign `$`
4. dot `.`
5. pipe `|`
6. question mark `?`
7. asterisk `*`
8. plus sign `+`
9. opening parenthesis `(`
10. closing parenthesis `)`
11. opening square bracket `[`
12. opening curly brace `{`

> If you want to use any of these characters as a literal in a regex, you need to escape them with a **backslash**. If you want to match `1+1=2`, the correct regex is `1\+1=2`. Otherwise, the plus sign has a special meaning.

## Non-Printable characters

Use `\t` to match a tab character (ASCII 0x09), `\r` for carriage return (0x0D) and `\n` for line feed (0x0A).
