# Function

## Named functions

- Named functions are declared with the keyword **`fn`**
- When using **arguments**, you **must declare data types**.
- By default, functions **return empty tuple`()`**. If you want to return a value, the **return type must be specified** after **`->`**

```rs
fn main() {
  println!("Hello, world!");
}

// Passing arguments.
fn print_sum(a: i8, b: i8) {
  println!("sum is: {}", a + b);
}

// Returning values
// 01. Without the return keyword.
// Only last expression returns.
fn plus_one(a: i32) -> i32 {
  a + 1 // NOTE: no ; semicolon.
  // If not using return then semicolon
  // must not be used.
  // It means this is an expression which
  // equals to `return a+1;
}

// 02. With the return keyword.
fn plus_two(a: i32) -> i32 {
  // Returns a+2. But, this's a bad practice.
  // Should use only on conditional returns,
  // except in the last expression.
  return a + 2;
}
```

## Assign function to variable

> Function pointers.

```rs
fn plus_one(a: i32) -> i32 {
  a + 1
}

// 1. Without type declarations
let b = plus_one;
let c = b(5); //6

// 2. With type declarations
let b: fn(i32) -> i32 = plus_one;
let c = b(5); //6
```

## Closures - anonymous functions - lambda functions

> The data types of arguments and returns are optional.

```rs
// Lets rewrite named function below
// as anonymous function
fn get_square_value(x: i32) -> i32 {
  x * x
}

//# 1. With argument type and return type

let x = 2;
// Input parameters are passed inside | | and
// expression body is wrapped within { }.
let square = |x: i32| -> i32 {
  x * x
};
println!("{}", square(x));

//# 2. Without argument type and return type

let x = 2;
// { } are optional for single-lined closures.
let square = |x| x * x;
println!("{}", square(x));
```
