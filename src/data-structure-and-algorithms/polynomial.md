# Polynomial

Polynomial comes from poly- (meaning "many") and -nomial (in this case meaning "term") ... so it says "many terms"

!!! example
    ![Polynomial](assets/polynomial-example.svg)

    Polynomial which has 3 terms.

There are special names for polynomials with 1, 2 or 3 terms:

- **Monomial:** one term. Example, 3, 3x, y^2^, 3xy^4^
- **Binomial:** two terms. Example, 3 + 3xy^4^
- **Trinomial:** three terms. Example, 3 + 3xy^4^ + 2x

## Elements of polynomial

1. **Constant:** 3, -20
2. **Variable:** x, y
3. **Exponent:** 2 in y^2^. It must be **positive** integers, 0, 1, 2, 3 etc.
4. **Coefficient:** It is a multiplicative factor of term in polynomial (attached to the variable). For example in, $7x^2 - 3xy + 1.5 + y$. The first two terms respectively have the coefficients 7 and −3. The third term 1.5 is called constant. The final term does not have any explicitly written coefficient, but is considered to have coefficient 1, since multiplying by that factor would not change the term.
5. **Operations:** addition, subtraction, multiplication and division (but not division by variable, so something like 2/x is not allowed).

## is polynomial

| Polynomial  | ?                                |
|-------------|----------------------------------|
| 5           | yes, one term is allowed         |
| 3x          | yes                              |
| 7x^2^ - 3xy | yes                              |
| 3x^-2^      | no, because negative exponent    |
| 1/x         | no, because division by variable |

## Like terms

The terms whose variables are the same.

!!! example
    - 7x, -2x
    - 2x^2^, x^2^
    - xy, 2xy
    - 2x^2^, 2x _[no]_
    - 2x^2^y, 2x _[no]_

## Adding polynomials

1. Place like terms together.
2. Add like terms.

!!! example
    $(2x^2 + 6x + 5)$ and $(3x^2 - 2x - 1)$

    - $(2x^2 + 3x^2) + (6x - 2x) + (5 - 1)$ _place like terms together_
    - $(2 + 3)x^2 + (6-2)x + (5 - 1)$
    - $5x^2 + 4x + 4$ _add like terms_

## Multiplying polynomials

1. Multiply each term in one polynomial by each term in the other polynomial.
2. Add those answers together, and simplify if needed.

!!! example
    - $–3x(4x^2 – x + 10)$
    - $–3x(4x^2) + (–3x)(–x) + (–3x)(+10)$
    - $(-12)(x^{1 + 2}) + (3)(x^{1 + 1}) + (-30)(x)$
    - $-12x^3 + 3x^2 -30x$

## Degree

The highest power (exponent) of variable in the polynomial. For example, in polynomial $x^2 + 2x + 4$, the degree is 2.

### How to find degree

$$3x^2 - 3x^4 - 5 + 2x + 2x^2 + x$$

1. Combine like terms.
    - $(3x^2 + 2x^2) + (- 3x^4) + (-5) + (2x + x)$
    - $(3 + 2)(x^2) + (- 3x^4) + (-5) + (2 + 1)(x)$
    - $5x^2 - 3x^4 - 5 + 3x$
2. Drop all of the constants and coefficients.
    - $x^2 - x^4 + x$.
3. Put the terms in decreasing order of their exponents. This is also called putting the polynomial in **standard form**.
    - $-x^4 + x^2 + x$. Now 4 is the highest exponent and which is a degree of polynomial.

!!! info "degree of constant is 0"
    15 can be written as $15x^0$ = 0 degree

!!! info "degree of variable with no exponent is 1"
    $15x$ can be written as $15x^1$ = 1 degree

#### Degree with multiple variables

$$x^5y^3z + 2xy^3 + 4x^2yz^2$$

Can be written as:

$$x^5y^3z^1 + 2x^1y^3 + 4x^2y^1z^2$$

1. Add up the degrees of the variables in each of the terms; it does not matter that they are different variables.
    - deg($x^5y^3z^1$) = 5 + 3 + 1 = 9
    - deg($2x^1y^3$) = 1 + 3 = 4
    - deg($4x^2y^1z^2$) = 2 + 1 + 2 = 5
2. The largest degree of these three terms is 9.
