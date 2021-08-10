# Character classes or sets

With a "character class", also called "character set", you can tell the regex engine to match **only one out of several characters.**

> **Square brackets:** Simply place the characters you want to match between square brackets.

If you want to match an `a` or an `e`, use `[ae]`. You could use this in `gr[ae]y` to match either `gray` or `grey`.

* A character class matches only a **single character.** `gr[ae]y` does not match `graay`, `graey` or any such thing.

* **Hyphen** inside a character class to specify a range of characters. `[0-9]` matches a single digit between 0 and 9. You can use more than one range such as `[0-9a-fA-F]`, `[0-9a-fxA-FX]`.

## Shorthand character classes

1. `\d` = `[0-9]`
2. `\D` = Any non digit character.
3. `\w` = `[A-Za-z0-9_]`. Any Alphanumeric character.
4. `\W` =  Any non Alphanumeric character.
5. `\s` = `[ \t\r\n\f]`. Matches space, a tab, a line break, or a form feed.
6. `\S` = Any non white space character.

> Shorthand character classes can be used both **inside** and **outside** the square brackets. `\s\d` matches a whitespace character followed by a digit. `[\s\d]` matches a single character that is either whitespace or a digit.

## Negation

Typing a **caret** after the opening square bracket negates the character class. The result is that the character class matches any character that is not in the character class. `[^0-9]` matches any character that is not a digit.

## Repetition

If you repeat a character class by using the `?`, `*` or `+` operators, you're repeating the entire character class. You're not repeating just the character that it matched. The regex `[0-9]+` can match `837` as well as `222`.

## Subtraction

**Syntax:** `[class-[subtract]]`
Match any single character present in one list (the character class), but not present in another list (the subtracted class).

Character after a hyphen is an opening bracket, these flavors interpret the hyphen as the subtraction operator rather than the range operator.

> Character class subtraction syntax is incompatible with Perl and the majority of other regex flavors.

**Example:** The character class `[a-z-[aeiuo]]` matches a single letter that is not a vowel. Match any character from `a` to `z` except `a, e, i, u, o`.

* The class subtraction must always be the **last** element in the character class. `[0-9-[4-6]a-f]` is not a valid regular expression. It should be rewritten as `[0-9a-f-[4-6]]`. The subtraction works on the **whole class**.

### Negation takes precedence over subtraction

The character class `[^1234-[3456]]` is both negated and subtracted from. This class should be read as "(not 1234) minus 3456". Thus this character class matches any character other than the digits 1, 2, 3, 4, 5, and 6.
