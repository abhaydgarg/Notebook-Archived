# Generics

Sometimes, when writing a function or data type, we may want it to work for multiple types. In Rust, we can do this with generics.

> Generic - Can be any type - Work with multiple types.

```rs
// Vector is a generic type which can works with:

// type i32
let mut a1: Vec<i32> = vec![];
// type u64
let mut a2: Vec<u64> = vec![];
// type string
let mut a2: Vec<&str> = vec![];
// and many more...
```

The concept is, we take upercase letter like `T`, `U` etc and use it by wrapping with `<>` which tells the compiler that it is a generic type (can be any type).

```rs
//# Generic functions
//1. x has type T, T is a generic type
fn takes_anything<T>(x: T) {
}
//2. Both x and y has the same type
fn takes_two_of_the_same_things<T>(x: T, y: T) {
}
//3. Multiple types
fn takes_two_things<T, U>(x: T, y: U) {
}
```

```rs
//# Generic structs
// Declares a generic structure named Data.
// The <T> type indicates any data type. The main()
// function creates two instances − an integer instance
// and a string instance, of the structure.
struct Data<T> {
  value: T,
}

fn main() {
  // type of i32
  let t1: Data<i32> = Data { value: 350 };
  println!("value is :{} ",t1.value);
  //value is :350

  //type of String
  let t2: Data<String> = Data { value:"Tom".to_string() };
  println!("value is :{} ",t2.value);
  // value is :Tom
}

// Another example
struct Point<T> {
  x: T,
  y: T,
}

fn main() {
  // T is a int type
  let point_a = Point { x: 0, y: 0 };
  // T is a float type
  let point_b = Point { x: 0.0, y: 0.0 };
}
```

## Remove duplication

Let's say you want to have function that add two numbers. The type of both numbers either `int` or `float`.

```rs
fn main() {
  println!("{}", add_int(1, 2));
  // 3
  println!("{}", add_float(1.1, 2.1));
  // 3.2
}

// To add integer type
fn add_int(a: i32, b: i32) -> i32 {
  a + b
}

// To add float type
fn add_float(a: f64, b: f64) -> f64 {
  a + b
}
```

In above example, we have 2 function to perfom addition of two numbers. One for integer type and another for float type. Using generic function we can have only one add function that handle both integer and float types.

```rs
use std::ops::Add;

fn main() {
  println!("{}", add(1, 2));
  // 3
  println!("{}", add(1.1, 2.1));
  // 3.2
}

// One function to add integer and float
fn add<T: Add<Output=T>>(a: T, b: T) -> T {
  a + b
}
```
