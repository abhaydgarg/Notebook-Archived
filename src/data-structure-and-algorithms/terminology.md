# Terminology

## Subarray/Substring

Contiguous and has order.

$\frac{n(n+1)}{2}$ which is sum of arithmetic series and has O(n^2^) time complexity.

```text
Input: {1, 2, 3}

Output: {1}, {2}, {3}, {1, 2}, {2, 3}, {1, 2, 3}
```

## Subsequence

Non-contiguous but has order.

$(2^n)-1$

```text
Input: {1, 2, 3}

Output: {1}, {2}, {3}, {1, 2}, {1, 3}, {2, 3}, {1, 2, 3}
```

See {1, 3} where 2 is missing that means it is non-contiguous (no continuity) but maintain the order that is 3 comes after 1.

## Subset

It is subsequence plus it has empty set.

$2^n$

```text
Input: {1, 2, 3}

Output: {1}, {2}, {3}, {1, 2}, {1, 3}, {2, 3}, {1, 2, 3}, {}
```

## Powerset

A Powerset is a set of all the subsets of a set.

Power Set of {a, b, c}

P(S) = { {}, {a}, {b}, {c}, {a, b}, {a, c}, {b, c}, {a, b, c} }

!!! note
    Empty set is also a part of powerset.

!!! question "How many subsets?"
    $2^n$ subsets.

    {a, b, c} => $2^3$ = 8

!!! info "Interesting"
    - subsets = powerset.
    - subsequences = powerset excluding empty set.

### It's Binary

Let's take an example {1, 2, 3}.

**[Step 1]:** Generate binary representation of all numbers from 0 to 2^3^ - 1 .

```
0 = 000
1 = 001
2 = 010
3 = 011
4 = 100
5 = 101
6 = 110
7 = 111
```

**[Step 2]:** Determine from the binary representation whether or not to include a number from the set.

```
000 = [exclude, exclude, exclude] = []
001 = [exclude, exclude, include] = [3]
010 = [exclude, include, exclude] = [2]
011 = [exclude, include, include] = [2, 3]
100 = [include, exclude, exclude] = [1]
101 = [include, exclude, include] = [1, 3]
110 = [include, include, exclude] = [1, 2]
111 = [include, include, include] = [1, 2, 3]
```

## Permutation vs combination

In permutation order matters. Therefore, Alice, Bob and Charlie is different from Charlie, Bob and Alice.

In combination order does not matter. Therefore, Alice, Bob and Charlie is the same as Charlie, Bob and Alice.

> A joke: A "combination lock" should really be called a "permutation lock". The order you put the numbers in matters.
