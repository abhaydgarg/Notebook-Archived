# Logical operators

## AND

Control enters in conditional block only when TRUE.

Assume that the number 1 is true, and 0 is false. Here is a truth table for the logical && operator.

| a | b | Result |
|---|---|--------|
| 0 | 0 | 0      |
| 0 | 1 | 0      |
| 1 | 0 | 0      |
| 1 | 1 | 1      |

* Both must be true then ONLY TRUE.
* If find first value false then engine does not check second one because at the first place value is false, so there is no need to check further.
* If find first value true then engine checks second one because in order to be true as whole the second value must be true as well.
* In other words, **engine keeps checking until gets false.**

## OR

The \|\| operator works slightly differently.

| a | b | Result |
|---|---|--------|
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

* Any is true then TRUE.
* If find first value false then engine checks second one because at the first place value is false, so there is need to check further for true value.
* If find first value true then engine does not check second one because at the first place value is true, so there is no need to check further.
* In other words, **engine keeps checking until gets true.**
