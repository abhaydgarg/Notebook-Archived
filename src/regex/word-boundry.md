# Word boundaries

`\b` - Match word boundary.

Simply put: `\b` allows you to perform a "whole words only" search using a regular expression in the form of `\bword\b`.

## Which characters are word characters?

Characters that are matched by the short-hand character class `\w` are the characters that are treated as word characters by word boundaries. `\w` stands for "word character" and always matches the ASCII characters `[A-Za-z0-9_]`.

All characters that are not "word characters" means `[A-Za-z0-9_]` are "non-word characters".

The regex `\bcat\b` would therefore match cat in a `black cat`, but it wouldn't match it in `catatonic`, `tomcat` or `certificate`. Removing one of the boundaries, `\bcat` would match cat in `catfish`, and `cat\b` would match cat in `tomcat`.

> **`\b` just means non word character.**

## Inside the regex engine

Lets see how regex handles `\b`, a word boundary.

**Regex** = `\bis\b`
**String** = `This island is beautiful`

The engine starts with the first token `\b` at the first character `T`. Since this token is zero-length, the position before the `T` character is inspected which is void, so `\b` matches here. The engine continues with the next token `i` which try to match `T` and failed. Now start again using token `\b`; the `\b` cannot match `h`, `i`, and `s` because they are word characters.

The next character in the string is a `space`. `\b` matches here because the space is not a word character. Continuing, the regex engine finds that `i` matches `i` and `s` matches `s`. Now, the engine tries to match the second `\b` at the position before the `l`. This fails because `\b` found `l` which is word character and not a non word character.

Engine continues again and now `\b` matches at the position before the third `i` in the string. The engine continues, and finds that `i` matches `i` and `s` matches `s`. The last token in the regex, `\b`, also matches the space in a string because the space is not a word character. The engine has successfully matched the word `is` in our string.
