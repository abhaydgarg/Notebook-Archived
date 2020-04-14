# Float number gotcha

## Why 0.1+0.2 IS NOT equal to 0.3

It does not happen in one language, but this how float numbers are handled in binary.

> Be careful when dealing with float numbers, because operations (addition, subtraction, equality check etc) on float numbers are not **always** 100% accurate. You may get unexpected result.

This is not caused by the programming language itself, but by the way real numbers are **stored** in a computer, using what is called **floating-point number** representation.

We're actually confronted with a similar problem when using pen and paper, using our beloved decimal (**base**  **10**) notation.
If we want to write down the value of 1/10, it's very easy, we just write "0.1".
If we want to write down the value of 1/3, we have to write "0.33333333.." ad infinitum. We say that a fraction like 1/10 **terminates** in base 10, whereas fractions like 1/3 **don't terminate** in base 10.
When writing down 0.333333.... if we stop at any point then what we get is an approximation of the real value. If the last digit we write is a 3, then our approximation is slightly below the real value, and if we instead stop with a 4 (eg. 0.333333334) the approximation is slightly above the real value. If we wanted to store this value on a piece of paper, then we'd have to choose either one of these approximations. If we then were to do arithmetic operations using such values, then the **errors** introduced by each approximation would add up. For instance, if we were asked to add 1/3 and 2/3, what we would get is:

```
0.333333+0.666666=0.999999
```

instead of 1.0 that we could expect.

Now a computer uses binary (**base 2**) notation to store numbers. It happens that in this base, neither 1/3 nor 1/10 terminate.
(According to the Wikipedia page on binary numbers, "_Fractions in binary only terminate if the denominator has 2 as the only prime factor_", which means that there's a lot more fractions that don't terminate, compared to base 10. For instance, 1/5 and 1/6 also don't terminate.)
When you write a program, you're using **base 10** as an input but the numbers are stored in **base 2**:

- what you write in your source code: 0.1+0.2

- what the computer stores: 0.000110011... + 0.00110011...

Because of the errors introduced by the approximation for 0.1 and 0.2 (none of them actually terminates), then the result won't make **exactly** 0.3 (which is stored as 0.01001 in binary, by the way). Which is why when we write programs we almost never use equality check between floating-point numbers, but epsilon-absolute error, ie. we test that the difference between the two approximations is less than a very small amount. In your case we would write something like:

```
a = 0.1+0.2
b = 0.3
if (abs(a-b)<0.0000001):
print "a is equal to b"
```
