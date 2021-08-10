# Sort algorithm

## What is stable sort?

The stability of a sorting algorithm is concerned with how the algorithm treats equal (or repeated) elements. Stable sorting algorithms preserve the relative order of equal elements, while unstable sorting algorithms don't. In other words, stable sorting maintains the position of two equals elements relative to one another.

Let's understand the concept with the help of an example. We have an array of integers A:  [ 5, 8, 9, 8, 3 ]. Let's represent our array using color-coded balls, where any two balls with the same integer will have a different color which would help us keep track of equal elements (8 in our case):

![Stable and unstable](assets/Stable-vs-Unstable.png)

Stable sorting maintains the order of the two equal balls numbered 8, whereas unstable sorting may invert the relative order of the two 8s.

### Example

Consider a list of people. I may only want to sort them by their names, rather than all of their information. With a stable sorting algorithm, I know that if I have two people named "John Smith", then their relative order is going to be preserved.

```
Last     First       Phone
-----------------------------
Wilson   Peter       555-1212
Smith    John        123-4567
Smith    John        012-3456
Adams    Gabriel     533-5574
```

Since the two "John Smith"s are already "sorted" (they're in the order I want them), I won't want them to change positions. If I sort these items by last, then first with an unstable sorting algorithm, I could end up either with this:

```
Last     First       Phone
-----------------------------
Adams    Gabriel     533-5574
Smith    John        123-4567
Smith    John        012-3456
Wilson   Peter       555-1212
```

Which is what I want, or I could end up with this:

```
Last     First       Phone
-----------------------------
Adams    Gabriel     533-5574
Smith    John        012-3456
Smith    John        123-4567
Wilson   Peter       555-1212
```

(You see the two "John Smith"s have switched places). This is NOT what I want.

If I used a stable sorting algorithm, I would be guaranteed to get the first option, which is what I'm after.
