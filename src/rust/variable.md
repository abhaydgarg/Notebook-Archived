# Variable bindings

## Variable bindings `let`

> Variables are **immutable** by default, so we call them **Variable bindings**. To make them mutable, the `mut` keyword is used.

Rust is a **statically typed** language; it checks data types at compile-time. But it **doesn’t require you to actually type it when declaring variable bindings**. In that case, the compiler checks the usage and sets a better data type for it.

```rs
// Without data type - Compiler sets it for you
// based on usage.
let a = true;

// Explicitly set data type.
let b: bool = true;

// Mutable variable
let mut z = 5;
z = 6;

// Multiple variables
let (x, y) = (1, 2);
// Multiple variables mutable
let (mut x, mut y) = (1, 2);
// Multiple variables with types
let (x, y) : (i32, i32) = (1, 2);
```

### Declare first

It's possible to declare variable bindings first, and initialize them later. However, this form is seldom used, as it may lead to the use of uninitialized variables.

The compiler forbids use of uninitialized variables, as this would lead to undefined behavior.

```rs
fn main() {

  let a_binding;

  // Error! Use of uninitialized binding.
  // Because using it before initializing.
  println!("a binding: {}", a_binding);

  // Initialized.
  a_binding = 1;

  // Ok
  println!("a binding: {}", a_binding);
}
```

## Scope

Variable bindings have a scope, and are constrained to live in a _block_. A block is a collection of statements enclosed by braces `{}`.

```rs
fn main() {
  // This binding lives in the main function
  let long_lived_binding = 1;

  {
      // This binding only exists in this block
      let short_lived_binding = 2;
      println!("inner short: {}", short_lived_binding); // inner short: 2
      let long_lived_binding = 5_f32;
      println!("inner long: {}", long_lived_binding); // inner long: 5
  }

  // Error! `short_lived_binding` doesn't exist in this scope
  println!("outer short: {}", short_lived_binding);

  println!("outer long: {}", long_lived_binding); // outer long: 1
}
```

### Cannot declare variable using `let` globally

```rs
let x = 1; // Error

fn main() {
  println!("{}", x);
}
```

## Mutability

Variable bindings are immutable by default, but this can be overridden using the `mut` modifier.

```rs
fn main() {
  let immutable_binding = 1;
  let mut mutable_binding = 1;

  // Ok
  mutable_binding = 2;

  // Error!
  immutable_binding = 2;
}
```

## Variable shadowing

You can declare a new variable with the same name as a previous variable, and the new variable shadows the previous variable. Rustaceans say that the first variable is shadowed by the second, which means that the second variable’s value is what appears when the variable is used. We can shadow a variable by using the same variable’s name and repeating the use of the `let` keyword.

Program below, first binds `x` to a value of `5`. Then it shadows `x` by repeating `let x =`, taking the original value and adding `1` so the value of `x` is then `6`. The third `let` statement also shadows `x`, multiplying the previous value by `2` to give `x` a final value of `12`.

```rs
fn main() {
  let x = 5;
  let x = x + 1;
  let x = x * 2;
  // The value of x is: 12
  println!("The value of x is: {}", x);
}
```

### Shadowing gotchas

> **TODO:** Find out why such behavior.
> **Guess:** Before redeclaring again using `let` the variable must be used once.

```rs
//# Does not work.
fn main() {
  let x = 1;
  let x = 2; // Error
  println!("{}", x);
}

//# This works fine.
fn main() {
  let x = 5;
  let x = x + 1;
  let x = x * 2;
  println!("The value of x is: {}", x); // The value of x is: 12
}

//# This works fine.
fn main() {
  let x = 1;
  println!("{}", x); // 1
  let x = 2;
  println!("{}", x); // 2
}
```

### Difference between shadowing and immutability

See the example below, as in Rust the variables are immutable by default and can only be made mutable by `mut` keyword. But in shadowing, the variable `x` is changed by redeclaring and this seems breaking the immutability rule.

```rs
//# Shadowing
fn main() {
  let x = 5;
  println!("The value of x is {}", x);
  let x = 6;
  println!("The value of x is {}", x);
}

// The value of x is 5
// The value of x is 6

//# Mutability
fn main() {
  let mut x = 5;
  println!("The value of x is {}", x);
  x = 6;
  println!("The value of x is {}", x);
}

// The value of x is 5
// The value of x is 6
```

#### Explanation

The confusion is because we are conflating names with storage.

```rs
fn main() {
  let x = 5; // x_0
  println!("The value of x is {}", x);
  let x = 6; // x_1
  println!("The value of x is {}", x);
}
```

There is one name `x`, and two storage locations `x_0` and `x_1`. The second `let` is simply re-binding the name `x` which refer to storage location `x_1`. The `x_0` storage location is entirely unaffected.

```rs
fn main() {
  let mut x = 5; // x_0
  println!("The value of x is {}", x);
  x = 6;
  println!("The value of x is {}", x);
}
```

There is one name `x`, and one storage location `x_0`. The `x = 6` assignment is directly changing the bits of storage location `x_0`.

## If variable is mutable then nested variables or properties are automatically become mutable

Let's see an example of `struct`:

```rs
fn main() {
  // Declared unmutable.
  let a = C {
    v: 1,
  };

  // `s` is mutable but
  // `a` which is not define mutable
  // when declared above, automatically
  // becomes mutable after it contained in
  // `s`.
  let mut s = S::new(vec![a]);
  s.add(C {v: 2});
  s.update(3);

  println!("{:?}", s);
}

#[derive(Debug)]
struct C {
  v: u32
}

#[derive(Debug)]
struct S {
  x: Vec<C>
}

impl S {

  fn new(x: Vec<C>) -> S {
    S {
      x: x
    }
  }

  fn add(&mut self, val: C) {
    self.x.push(val);
  }

  fn update(&self, val: u32) {
    self.x[0].v = val;
  }
}
```
