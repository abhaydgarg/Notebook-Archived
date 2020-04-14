# Time complexity classes

1. Constant: O(1)
2. Logarithmic: O(log n)
3. Linear: O(n)
4. Linearithmic: O(n log n)
5. Quadratic: O(n^2^)
6. Cubic: O(n^3^)
7. Exponential: O(2^n^)
8. Factorial: O(n!)

![Time complexities graph](assets/big-o-running-time-complexity.png)

## Big O

### The basic idea

Big-O notation is a way to understand how code behaves as its data gets larger. To put it in rhyme form:

> Big-O: how code slows as data grows.

Imagine you have a chunk of code that does something to a bunch of data. You’d like to know how much slower the code will go if you give it ten times more data. That’s what big-O describes. You might imagine that the answer is of course, ten times slower. But that’s not the way real code works. One algorithm might go a hundred times slower. Another might not go any slower at all. Big-O is a way to analyze the code and determine the slow-down factor.

1. How **time** grows when data grows.
2. How **space** grows as data grows.

### How to calculate time complexity

#### Rules

1. Nested loops are multiplied together.
2. Sequential loops are added.
3. Constants and cofficients are dropped.
4. Drop the less significant terms. In other words, find the degree of the polynomial.
5. Use different terms for different inputs.

```js
// Size of an array is `n`
function total(arr) {
  let totalNumber = 0;
  for (let i=0; i<arr.length; i++) {
     totalNumber += arr[i];
  }
  return totalNumber;
}
```

| Line# | Statement              | Time                                                                                   |
|-------|------------------------|----------------------------------------------------------------------------------------|
| 3     | let totalNumber = 0;   | 1                                                                                      |
| 4     | let i = 0;             | 1 (execute once)                                                                       |
| 4     | i < arr.length;        | n + 1 (when i reaches n+1, the loop stops. So one more step to fail the condition)     |
| 4     | i++                    | n                                                                                      |
| 5     | totalNumber += arr[i]; | Inside for loop, so it executes "no of times for loop executes multiply by 1" => n x 1 |
| 7     | return totalNumber;    | 1                                                                                      |

!!! note ""
    * T = 1 + 1 + (n + 1) + n + n(1) + 1
    * T = 4 + n + n + n
    * T = 4 + 3n
    * T = n
    * T = O(n)

#### Why nested loops are multiplied

Lets take an example.

```js
for(let i = 0; i < n; i++) {
  for(let j = 0; j < n; j++) {
  }
}
```

Let's say we have n=5

| Outer loop  | Inner loop |
|-------------|------------|
| iteration 1 | 5 times    |
| iteration 2 | 5 times    |
| iteration 3 | 5 times    |
| iteration 4 | 5 times    |
| iteration 5 | 5 times    |

Total work can be obtained by **adding number of times a inner loop runs for each iteration.**

- 5 + 5 + 5 + 5 + 5 and this can be written as
- 5 x 5 _[number of times outer loop runs X number of times inner loop runs]_
- 5^2^ => O(n^2^)

In this example, number of times inner loop run does not change for each iteration of outer loop.

Let's take an another example.

```js
for(let i = 0; i < n; i++) {
  for(let j = 1; j < n; j++) {
  }
}
```

Let's say we have n=5

| Outer loop  | Inner loop |
|-------------|------------|
| iteration 1 | 4 times    |
| iteration 2 | 4 times    |
| iteration 3 | 4 times    |
| iteration 4 | 4 times    |
| iteration 5 | 4 times    |

Here again, **number of times inner loop runs does not change for each iteration of outer loop.** So we can simply represent it as 5 x 4. More or less it has time complexity O(n^2^).

##### What if number of times inner loop run changes for each iteration of outer loop

Here is a example where we want to print all the substrings of a string.

```js
function printSubstrings(str) {
  let result = [];
  for (let i = 0; i < str.length; i++) {
    let substring = '';
    for (let j = i; j < str.length; j++) {
      substring += str[j];
      result.push(substring);
    }
  }
  print(result);
}
printSubstrings('abcd');
// [ 'a', 'ab', 'abc', 'abcd', 'b', 'bc', 'bcd', 'c', 'cd', 'd' ]
```

For n=4

| Outer loop  | Inner loop | substrings       |
|-------------|------------|------------------|
| iteration 1 | 4 times    | a, ab, abc, abcd |
| iteration 2 | 3 times    | b, bc, bcd       |
| iteration 3 | 2 times    | c, cd            |
| iteration 4 | 1 times    | d                |

After each iteration of outer loop, number of times inner loop runs is reduced by 1. Here we cannot obtain work done by doing _[number of times outer loop runs X number of times inner loop runs]._ Because we do not have fixed number for inner loop. In this case we fallback to addition and obtain the total work done by **adding number of times a inner loop runs for each iteration.**

- 4 + 3 + 2 + 1 which is an arithemtic series.
- $\frac{n(n+1)}{2}$ gives a sum of arithmetic series and has O(n^2^) time complexity approximately.

##### Examples

###### [1] Nested loop

```js
// Nested loops are multiplied together.
for(let i = 0; i < n; i++) {
  for(let j = 0; j < n; j++) {
  }
}
```

!!! note ""
    * T = (1 + (n + 1) + n) x (1 + (n + 1) + n)
    * T = (2 + 2n) x (2 + 2n)
    * T = 4 + 4n + 4n + 4n^2^
    * T = 4 + 8n + 4n^2^
    * T = n + n^2^
    * T = O(n^2^)

###### [2] Sequential loops

```js
// Sequential loops are added.
for(let i = 0; i < n; i++) {
}

for(let i = 0; i < n; i++) {
  for(let j = 0; j < i; j++) {
  }
}
```

!!! note ""
    * First loop T = n
    * Second nested loop T = n^2^
    * You may calculate T = n + n^2^ => O(n + n^2^) which is wrong both loops are running on single input "n" and we only want largest term for single input. Hence T = O(n^2^)

###### [3] Different inputs

```js
// Different terms for inputs.
runItTwice = (items, moreItems) => {
  items.forEach(function (items) {
    console.log(items);
  });

  moreItems.forEach(function (items) {
    console.log(items);
  });
}
```

!!! note ""
    T = O(a + b), Where `a` represents `items` and `b` represents `moreItems`.
    Here, drop less significant term does not apply, because even though both loops are sequential but they are running on different inputs. If they run on single input say "n" then we keep largest term.

###### [4] Increment by 2*i

```js
// Incremented by 2*i in each run.
for(let i = 1; i < n; i *= 2) {
}

// for loop can also be written in while loop.
let i = 1;
while(i < n) {
  i *=2;
}
```

!!! note ""
    * T = 1 + (x + 1) + x
    * T = 2 + 2x
    * T = x

    Since the loop is incremented by `2*i` each time it runs, the condition `i < n` is not going to run n times. Lets say the loop runs `x` times then the condition checking is done `x + 1` times.

    Now lets find out x.

We cannot find "x" just by looking, so lets find out the pattern by which "i" is increasing.

1. i = 1
2. i = 1 x 2 = 2
3. i = 2 x 2 = 4
4. i = 4 x 2 = 8
5. i = 8 x 2 = 16
6. i = 16 x 2 = 32

Pattern = 2^0^, 2^1^, 2^2^, 2^3^, 2^4^, 2^5^ ... 2^x^

Lets assume that loop is going to stop when i = 2^x^. And we do not know the value of x. The value of x is the number of times loop is going to run.

We have loop termination condition i < n, so we can say that (replace i with 2^x^):

2^x^ < n => 2^x^ = n

Above expression is an exponent expression and we have the value of "n" before hand in our program and the base which is 2. So to find out the "x", the power, we have to inverse the exponent expression. And the inverse of exponent is logarithim.

$2^x = n$

The value of x would be:

$x = \log_2 n$

$T = O(\log n)$

###### [5] Nested loop with different inputs

```js
for(let i=0; i<m; i++) {
  for(let j=0; j<n; j++) {
  }
}
```

!!! note ""
    * T = (1 + (m + 1) + m) x (1 + (n + 1) + n)
    * T = (2 + 2m) x (2 + 2n)
    * T = 4 + 4n + 4m + 4mn
    * T = n + m + mn
    * T = O(mn)

###### [6] Tricky one

```js
for(let i=4*n; i>=1; i--) {
}
```

!!! note ""
    * T = 1 + (n + 1) + n
    * T = O(n)

    There is a trick, you have multiply n by 4. See it does not make a difference if n is 10 or 100. Since the loop is incrementing by 1, the time complexity will always be linear.

###### [7] Increment by 3*i

```js
// Increment by 3*i.
for(let i = 1; i < n; i *= 3) {
}
```

!!! note ""
    * T = 1 + (x + 1) + x
    * T = 2 + 2x
    * T = x

    Since the loop is incremented by `3*i` each time it runs, the condition `i < n` is not going to run n times. Lets say the loop runs `x` times then the condition checking is done `x + 1` times.

    Now lets find out x.

We cannot find "x" just by looking, so lets find out the pattern by which "i" is increasing.

1. i = 1
2. i = 1 x 3 = 3
3. i = 3 x 3 = 9
4. i = 9 x 3 = 27
5. i = 27 x 3 = 81
6. i = 81 x 3 = 243

Pattern = 3^0^, 3^1^, 3^2^, 3^3^, 3^4^, 3^5^ ... 3^x^

Lets assume that loop is going to stop when i = 3^x^. And we do not know the value of x. The value of x is the number of times loop is going to run.

We have loop termination condition i < n, so we can say that (replace i with 3^x^):

3^x^ < n => 3^x^ = n

Above expression is an exponent expression and we have the value of "n" before hand in our program and the base which is 3. So to find out the "x", the power, we have to inverse the exponent expression. And the inverse of exponent is logarithim.

$3^x = n$

The value of x would be:

$x = \log_3 n$

$T = O(\log n)$

!!! important
    Base does not matter, whether 2, 3, 4, 10. It just says that the time increases logarithmically as n increases.

###### [8] Square root

```js
for(let i=1; i<=n; i++) {
  i = i*i;
}
```

!!! note ""
    * $T = 1 + (x + 1) + x + x$
    * $T = 2 + 3(x)$
    * $T = x$

    Lets find out x.

1. i = 1
2. i = 2 x 2 = 4
3. i = 5 x 5 = 25
4. i = 26 x 26 = 676

Pattern = $2^2, 5^2, 26^2 ... x^2$.

The loop terminates when:

$x^2 = n$

$x = \sqrt{n}$

$T = O(\sqrt{n})$

###### [9] Exponential O(2^n^)

For example, **Subsets of a Set**. Finding all distinct subsets of a given set.

```js
// Note: there is one empty item too before a ['', a]
getSubsets('a') // , a...
// n = 1, f(n) = 2;
getSubsets('ab') // , a, b, ab...
// n = 2, f(n) = 4;
getSubsets('abc') // , a, b, ab, c, ac, bc, abc...
// n = 3, f(n) = 8;
getSubsets('abcd') // , a, b, ab, c, ac, bc, abc, d, ad, bd, abd, cd, acd, bcd...
// n = 4, f(n) = 16;
getSubsets('abcde') // , a, b, ab, c, ac, bc, abc, d, ad, bd, abd, cd, acd, bcd...
// n = 5, f(n) = 32;
```

Did you notice any pattern? Every time the input gets longer the output is twice as long as the previous one.

###### [10] Factorial O(n!)

Factorial is the multiplication of all positive integer numbers less than itself. For instance:

```
5! = 5 x 4 x 3 x 2 x 1 = 120

It grows pretty quickly:

20! = 2,432,902,008,176,640,000
```

For example, **Permutations**. Function that computes all the different words that can be formed given a string.

```js
getPermutations('ab') // ab, ba...
// n = 2, f(n) = 2;
getPermutations('abc') // abc, acb, bac, bca, cab, cba...
// n = 3, f(n) = 6;
getPermutations('abcd') // abcd, abdc, acbd, acdb, adbc, adcb, bacd...
// n = 4, f(n) = 24;
getPermutations('abcde') // abcde, abced, abdce, abdec, abecd, abedc, acbde...
// n = 5, f(n) = 120;
```

### How to calculate space complexity

> Which variable grows as size of input grows.

```js
// O(1) space (we use a fixed number of variables)
function sayHiNTimes(n) {
  for (let i = 0; i < n; i++) {
    console.log('hi');
  }
}
```

```js
// O(n) space (the size of hiArray scales with the size of the input)
function arrayOfHiNTimes(n) {
  const hiArray = [];
  for (let i = 0; i < n; i++) {
    hiArray[i] = 'hi';
  }
  return hiArray;
}
```

```js
// Usually when we talk about space complexity, we're talking about
// additional space, so we don't include space taken up by the inputs.
// For example, this function takes constant space even though
// the input has n items.

function getLargestItem(items) {
  let largest = -Number.MAX_VALUE;
  items.forEach(item => {
    if (item > largest) {
      largest = item;
    }
  });
  return largest;
}
```

### Be reasonable

Understanding the algorithmic complexity of your code is a powerful tool. Choosing the right algorithm is the best way to make code go faster. But like any tool, big-O can be over-applied. It’s important, but it isn’t the only consideration.

For example, searching a list is O(n), but if the list is small, or if you only need to do it once, then who cares? Just write the simple code, and move on.

Another factor to consider is that big-O discards the coefficients. You might have two algorithms to choose between. One is O(n) and another is O(n^2^). Which is better? Well, if O(n) is n×1sec, and the O(n^2^) is n^2^×1msec, then the quadratic algorithm will be faster if n is less than 1000. You need to understand the complexity of the algorithm, but you also need to understand the actual range of your data, and then choose wisely.
