# Panicking

## panic!()

- In some cases, while an error happens we can not do anything to handle it, **if the error is something, which should not be happened**. In other words, if it’s an **unrecoverable error**.
- Also **when we are not using a feature-rich debugger or proper logs**, sometimes we need to **debug the code by quitting the program from a specific line of code** by printing out a specific message or a value of a variable binding to understand the current flow of the program.

> panic!() runs thread based. One thread can be panicked, while other threads are running.

```rs
//# Custom message
panic!("Error found");
panic!("Invalid number: {}", n);

//# Debug info
panic!("{:?}", data);
```

## assert!()

> Ensures that a boolean expression is true. It panics if the expression is false.

```rs
fn main() {
  let f = false;
  assert!(f)
}

// -------------- Compile-time error --------------
// thread 'main' panicked at 'assertion failed: f', src/main.rs:4:5
```

## assert_eq!()

> Ensures that two expressions are equal. It panics if the expressions are not equal.

```rs
fn main() {
  let a = 10;
  let b = 20;
  assert_eq!(a, b);
}

// -------------- Compile-time error --------------
// thread 'main' panicked at 'assertion failed: `(left == right)`
// left: `10`,
// right: `20`', src/main.rs:5:5

//# Custom message
assert_eq!(a, b, "a and b should be equal");
```

## assert_ne!()

> Ensures that two expressions are not equal. It panics if the expressions are equal.

```rs
fn main() {
  let a = 10;
  let b = 10;
  assert_ne!(a, b);
}

// -------------- Compile-time error --------------
// thread 'main' panicked at 'assertion failed: `(left != right)`
// left: `10`,
// right: `10`', src/main.rs:5:5
```
