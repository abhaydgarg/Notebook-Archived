# Repetitions

## Repetition

`*` - match the preceding token zero or more times.
`+` - match the preceding token once or more.
`{min,max}` - where min is zero or a positive integer number indicating the minimum number of matches, and max is an integer equal to or greater than min indicating the maximum number of matches.

```
{1} - exactly one, it may be any number

{0,1} - Optional, same as ?

{0,} - Infinite, same as *

{1,} - Atleast one, same as +
```

`\b[1-9][0-9]{3}\b` to match a number between 1000 and 9999.

## Greediness

`+`, `*` and `{min,max}` are greedy.

regex: `<.+>`
String: `This is a <EM>first</EM> test`

Expectation: It matches `<EM>` and `<EM>`.
Actually match: `<EM>first</EM>`

**Lets see inside engine.**

The first token in the regex is `<`. This is a literal. As we already know, the first place where it will match is the first `<` in the string. The next token is the dot, which matches any character except newlines. The dot is repeated by the plus. The plus is greedy. Therefore, the engine will repeat the dot as many times as it can.

The dot matches `E`, so the regex continues to try to match the dot with the next character. `M` is matched, and the dot is repeated once more. The next character is the `>`. You should see the problem by now. The dot matches the `>`, and the engine continues repeating the dot. The dot will match all remaining characters in the string. The dot fails when the engine has reached the void after the end of the string. Only at this point does the regex engine continue with the next token: `>`.

So far, `<.+` has matched `<EM>first</EM> test` and the engine has arrived at the end of the string. The engine remembers that the plus has repeated the dot more often than is required. Rather than admitting failure, the engine will **backtrack**. It will reduce the repetition of the plus by one, and then continue trying the remainder of the regex.

So the match of `.+` is reduced to `EM>first</EM> tes`. The next token in the regex is still `>`. But now the next character in the string is the last `t`. Again, these cannot match, causing the engine to backtrack further.
The total match so far is reduced to `<EM>first</EM> te`. But `>` still cannot match. So the engine continues backtracking until the match of `.+` is reduced to `EM>first</EM`. Now, `>` can match the next character in the string. The last token in the regex has been matched. The engine reports that `<EM>first</EM>` has been successfully matched.

> Remember that the regex engine is eager to return a match. It will not continue backtracking further to see if there is another possible match. It will report the first valid match it finds.

## Convert greediness to laziness

You can do that by putting a question mark `?` after the `+`, `*` and `{min, max}` in the regex.

Lets change the regex from example above to `<.+?>` and let see inside engine.

Again, `<` matches the first `<` in the string. The next token is the dot, this time repeated by a lazy plus. This tells the regex engine to repeat the dot as few times as possible. The minimum is one. So the engine matches the dot with `E`. The requirement has been met, and the engine continues with `>` and `M`. This fails.

Again, the engine will backtrack. But this time, the backtracking will force the lazy plus to expand rather than reduce its reach. So the match of `.+` is expanded to `EM`, and the engine tries again to continue with `>`. Now, `>` is matched successfully. The last token in the regex has been matched. The engine reports that `<EM>` has been successfully matched.

### Alternate ways of above problem without causing engine to backtrack

regex: `<[^>]+>`

The reason why this is better is because of the backtracking. When using the lazy plus, the engine has to backtrack for each character in the HTML tag that it is trying to match. When using the negated (`^`) character class, no backtracking occurs at all when the string contains valid HTML code. Backtracking slows down the regex engine.
