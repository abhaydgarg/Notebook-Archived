# Number system

In mathematics, a “base” or a “radix” is the number of different digits or combination of digits and letters that a system of counting uses to represent numbers.

## What is base

> It is the total number of distinct symbols used to represent numbers.

In decimal system (base 10), we can have unique numbers from 0 to $\infty$, so having distinct symbols for each number from 0 to $\infty$ is improbable. Therefore, we have 10 distinct symbols in base 10 to represent numbers from 0 to $\infty$. Similarly, in base 2 we have two distinct symbols (0 and 1) to represent any possible numbers.

!!! info "Types of base"
    - Base 10 (_Decimal)_ — Represent any number using 10 digits [0–9]
    - Base 2 (_Binary_) — Represent any number using 2 digits [0–1]
    - Base 8 (_Octal_) — Represent any number using 8 digits [0–7]
    - Base 16 (_Hexadecimal_) — Represent any number using 10 digits and 6 characters [0–9, A, B, C, D, E, F]

## So how do we build a number system

We all know how to write numbers up to 9, don’t we? What next then? Well, it’s simple really. When you have used up all of your symbols, what you do is,

- you add 1 digit to the left and make the right digit 0.
- When you used up all the symbols on both the right and left digit, then make both of them 0 and add another 1 to the left and it goes on and on like that.

!!! example
    - Write numbers 0–9.
    - Once you reach 9, make rightmost digit 0 and add 1 to the left which means 10.
    - Then on right digit, we go up until 9 and when we reach 19 we use 0 on the right digit and add 1 to the left, so we get 20.
    - Likewise, when we reach 99, we use 0s in both of these digits’ places and add 1 to the left which gives us 100.

You see when we have ten different symbols, each position is going to worth 10 times more than it’s previous one.

## What position says

### Decimal system (base 10)

Going to left, each position increase by 10 times because we have 10 distinct symbols to represent numbers.

!!! example
| 1     | 2     | 3     | 4     |
|-------|-------|-------|-------|
| 10^3^ | 10^2^ | 10^1^ | 10^0^ |
| 1000  | 100   | 10    | 1     |

    Number 1234 can be written as:

    - (1 x 10^3^) + (2 x 10^2^) + (3 x 10^2^) + (4 x 10^0^)
    - (1 x 1000) + (2 x 100) + (3 x 10) + (4 x 1)

### Binary system (base 2)

Going to left, each position increase by 2 times because we have 2 distinct symbols to represent numbers.

!!! example
| 1    | 0    | 0    | 1    |
|------|------|------|------|
| 2^3^ | 2^2^ | 2^1^ | 2^0^ |
| 8    | 4    | 2    | 1    |
