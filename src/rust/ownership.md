# Ownership

## Rules

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped. This is how memory is freed.

There are two kinds of type when talking about ownership, which are copy and move.

The functionality of a type is handled by the traits which have been implemented to it. By default,variable bindings have ‘move semantics.’ However, if a type implements `core::marker::Copy` trait, it has a ‘copy semantics’.

## Copy type

> Memory allocation **[stack]**

The types which are fixed in size are stored in stack. All data stored on the stack must have a known, fixed size.

Here are some of the types that are `Copy`:

- All the integer types, such as `u32`.
- The Boolean type, `bool`, with values `true` and `false`.
- All the floating point types, such as `f64`.
- The character type, `char`.
- Tuples, if they only contain types that are also `Copy`. For example, `(i32, i32)` is `Copy`, but `(i32, String)` is not.

### Points

- Mostly Primitive types, **by default** use copy mecahnism. That means the value of variable is copied to another variable. Both have their own copy and both are independent owners of their own copy and not connected to each other.
- When variable is assigned to another variable or pass it to the function as parameter then Rust make copy of owner variable's value and assign it to the another variable or function parameter.
- The ownership state of the original variable binding is set to “copied” state.

```rs
//# Variable assignment
fn main() {
  // Stack allocated integer
  let x = 5u32;
  // Copy `x` into `y`
  // No resources are moved
  // `x` is still an owner
  // `y` is also a independent owner having same
  // value as `x` that is `5`
  let y = x;
  // Both values can be independently used
  println!("x is {}, and y is {}", x, y);
}
// Output: x is 5, and y is 5

//# Pass variable as function parameter
fn main() {
  // Stack allocated integer
  let x = 5u32;
  // Copy - pass `x` to function test
  // no resources are moved
  // `x` is still an owner
  test(x);
  // Can access `x` because
  // it is still an owner
  println!("x: {}", x);
}

fn test(y: u32) {
  // `y` is also a owner having same
  // value as x that is `5`
  println!("y: {}", y);
}
// Output
// y: 5
// x: 5
```

## Move type

> Memory allocation **[heap]**

Data with a size that is unknown at compile time or a size that might change must be stored on the heap instead. For example `String` type.

### Points

- Non-primitive types, **by default** use move mechanism. It does not copy the data from one variable to another, rather it copy the memory address of variable which points to the data in heap to another variable and then unlink or disconnected the previous owner.
- When variable is assigned to another variable or pass it to the function as parameter then Rust move ownership to the another variable or function parameter and we can not access the original variable binding anymore.
- The ownership state of the original variable binding is set to “moved” state.

```rs
//# Variable assignment
fn main() {
  // Heap allocated string
  let s1 = String::from("hello");
  // Move `s1` into `s2`
  // resources are moved
  // `s1` is not an owner anymore
  // The pointer address of `s1` is copied (not the data) into `s2`.
  // `s2` now pointers to the heap allocated data, and
  // `s2` now an owner. `s1` is disconnected from data.
  let s2 = s1;
  // Error! `s1` can no longer access the data, because it no longer owns it
  println!("{}, world!", s1);
}

//# Pass variable as function parameter
fn main() {
  let s1 = String::from("hello");
  test(s1);
  // Error! `s1` can no longer access the data
  // `s2` owns it and `s2` is gone out of scope
  // here when `test` execution is ended. Since `s2`
  // was an owner not `s1`, so when `s2` go out of scope
  // memory is freed completely.
  println!("s1: {}", s1);
}

fn test(s2: String) {
  // `s2` is an owner now
  println!("s2: {}", s2);
} // `s2` is destroyed and the memory freed
  // at this point means when function execution
  // is ended after control passes `}`
```

## Give and take ownership

When ownership is moved to another variable then you can give it back by reassigning the variable.

```rs
fn main() {
  let mut s1 = String::from("hello");
  // Move ownership to function parameter a
  // get it back by reassigning
  s1 = test(s1);
  println!("s1: {}", s1);
}

fn test(a: String) -> String {
  println!("a: {}", a);
  a
}
```

> Taking ownership and then returning ownership with every function is a bit tedious. **What if we want to let a function use a value but not take ownership?** It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might want to return as well. Luckily for us, Rust has a feature for this concept, called **references and borrowing**.
