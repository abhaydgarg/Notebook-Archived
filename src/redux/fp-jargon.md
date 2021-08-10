# Functional programming jargon

## Pure function

> When function does not depend upon external state but just arguments passed. This way it always returns the same result if same arguments are passed everytime.

A function is pure if the return value is only determined by its input values (arguments), and does not rely on side effects to produce result.

### How about having local state in function?

Function can have its own local state for asisiting the computation of result. But the thing is that it is in total control of function and predictable unlike outside state.

### But function arguments are also come from outside state like from database or API?

Yes, it does not matter from where the arguments come from. The main point is "If same arguments are provided to function each time then we must always get a same result"

## Side effect

> Function has side-effect when it read/write from external state. For computing return value it depends upon the external state and not just argument passed.

A side effect is when a function relies on, or modifies, something outside its arguments to do something. For example, a function which reads or writes from a variable outside its own arguments, a database, a file can be described as having side effects.

Function with side-effect is not pure function because function has no control over the external state and you cannnot be sure to have same return value even if same arguments are passed everytime.

## Higher-Order Functions (HOF)

A function which takes a function as an argument and/or returns a function.

```js
const filter = (predicate, xs) => xs.filter(predicate)
const is = (type) => (x) => Object(x) instanceof type

filter(is(Number), [0, '1', 2, null]) // [0, 2]
```

## Arity

It is the number of arguments that function takes.

|         |                          |
|---------|--------------------------|
| 0-ary   | Nullary funciton         |
| 1-ary   | Unary (Monadic) funciton |
| 2-ary   | Binary funciton          |
| 3-ary   | Ternary funciton         |
| Varying | Variadic funciton        |

## Predicate function

> Boolean valued function

It is a function which either returns true or false value only.

## Variadic function

> Function that takes indefinite numbers of arguments.

It is a function of indefinite arity, i.e., one which accepts a variable number of arguments.

## Currying

The process of converting a function that takes multiple arguments into a function that takes them one at a time.

Each time the function is called it only accepts one argument and returns a function that takes one argument until all arguments are passed.

```js
const sum = (a, b) => a + b
const curriedSum = (a) => (b) => a + b

curriedSum(40)(2) // 42.
```

## Lambda

An anonymous function that can be treated like a value. For example, passed as an argument or returned from function.
