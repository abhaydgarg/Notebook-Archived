# Atomic grouping

An atomic group is a group that, when the regex engine exits from it, automatically throws away all backtracking positions remembered by any tokens inside the group.

Atomic groups are non-capturing.

**Syntax:** `(?>group)`

> Atomic grouping can be used to optimize regex engine search. See example below:

**Subject:** `integers`

## Without atomic grouping

**_regex:_** `\b(integer|insert|in)\b`

`\b` matches at the start of the string, and integer matches integer. The regex engine makes note that there are two more alternatives in the group, and continues with `\b`. This fails to match between the `r` and `s`. So the engine backtracks to try the second alternative inside the group. The second alternative matches `in`, but then fails to match `s`. So the engine backtracks once more to the third alternative. `in` matches `in`. `\b` fails between the `n` and `t` this time. The regex engine has no more remembered backtracking positions, so it declares failure.

This is quite a lot of work to figure out `integers` isn't in our list of words. We can optimize this by telling the regular expression engine that if it can't match `\b` after it matched `integer`, then it shouldn't bother trying any of the other words.

## With atomic grouping

**_regex:_** `\b(?>integer|insert|in)\b`

Now, when `integer` matches, the engine exits from an atomic group, and throws away the backtracking positions it stored for the alternation. When `\b` fails, the engine gives up immediately. This savings can be significant when scanning a large file for a long list of keywords. This savings will be vital when your alternatives contain repeated tokens.

## Don't be too quick to make all your groups atomic

Atomic grouping can exclude valid matches too.

Compare how `\b(?>integer|insert|in)\b`` and \b(?>in|integer|insert)\b` behave when applied to `insert`. The former regex matches, while the latter fails. If the groups weren't atomic, both regexes would match.
