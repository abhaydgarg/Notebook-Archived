# Lookaround

> Lookaround = Lookahead and Lookbehind.

Lookahead and lookbehind, collectively called "lookaround", are zero-length assertions just like the start and end of line, and start and end of word anchors. The difference is that lookaround actually matches characters, but then gives up the match, returning only the result: **match or no match**. That is why they are called "**assertions**". They do not consume characters in the string, but **only assert whether a match is possible or not**.

**With lookarounds, your feet stay planted on the string. You're just looking, not moving!**

| **Lookaround** | **Name**            | **What it Does**                                                                       |
|----------------|---------------------|----------------------------------------------------------------------------------------|
| (?=foo)        | Lookahead           | Asserts that what immediately follows the current position in the string is _foo_      |
| (?<=foo)       | Lookbehind          | Asserts that what immediately precedes the current position in the string is _foo_     |
| (?!foo)        | Negative Lookahead  | Asserts that what immediately follows the current position in the string is not _foo_  |
| (?<!foo)       | Negative Lookbehind | Asserts that what immediately precedes the current position in the string is not _foo_ |

## Example 1

Extract the Integer Portion of a Decimal Number, If and Only If it Has a Fractional Part.

**regex:** `\d+(?=\.\d+)`

The engine finds one or more digits, then checks for the existence of a decimal point and one-or-more digits. Since the lookahead here is positive, the match will only succeed if the engine finds a decimal part. Since lookahead is non-consuming, the engine has only matched, overall, the integer value of the double number.

## Example 2

Find a Word That Is NOT Preceded by Some Word or Phrase.

**regex:** `(?<!Hello )World`

The engine searches for the word "World".  Once it finds it, it begins evaluating the lookbehind. If it finds the string "Hello" and a trailing space, then the lookbehind fails, since it is a negative lookbehind.

## Example 3

Someone gave you a file full of film titles in **CamelCase**, such as **HaroldAndKumarGoToWhiteCastle**. To make it easier to read, you want to insert a space at each position between a lowercase letter and an uppercase letter.

**regex:** `(?<=[a-z])(?=[A-Z])`

The lookbehind asserts that what immediately precedes the current position is a lowercase letter. And the lookahead asserts that what immediately follows the current position is an uppercase letter.

> In most languages, when you feed this regex to the function that uses a regex pattern to split strings, it returns an array of words. And you can place space between.
