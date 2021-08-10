# Borrowing

Taking ownership and then returning ownership with every function is a bit tedious. **What if we want to let a function use a value but not take ownership?** It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might want to return as well. Luckily for us, Rust has a feature for this concept, called **references and borrowing**.

> Borrow but do not transfer ownership.

## Rules

1. Variable can be borrowed **either** as a shared borrow **or** as a mutable borrow **at a given time. But not both at the same time**.
2. Borrowing **applies for both copy types and move types**.

## Shared borrow [&]

- Read only.
- A variable can be borrowed single or multiple times, but data must not be altered.

```rs
//# Multiple borrow - Variable assignment
fn main() {
  let s = String::from("hello");
  let r1 = &s;
  let r2 = &s;
  println!("{}, {}", r1, r2);
  // hello, hello
}
```

```rs
//# Multiple borrow - as Function parameter
fn main() {
  let s1 = String::from("hello");
  // Read only `s1` borrow by `foo`
  foo(&s1);
  // Read only `s1` borrow by `bar`
  bar(&s1);
  // ownership is not moved from `s1`
  // so accessible here
  println!("main says: {}", s1);
}

fn foo (s2: &String) {
  println!("foo says: {}", s2);
}

fn bar (s2: &String) {
  println!("bar says: {}", s2);
}

// foo says: hello
// bar says: hello
// main says: hello
```

```rs
//# Altering - Gives an error
fn main() {
  let s1 = String::from("hello");
  // Read only borrow
  foo(&s1);
  println!("main says: {}", s1);
}

fn foo (s2: &String) {
  // Error! `s2` is a `&` reference, so the data
  // it refers to cannot be borrowed as mutable
  // Cannot write on read only reference or borrow
  s2.push_str(" world!");
  println!("foo says: {}", s2);
}
```

## Mutable borrow [&mut]

- Read and write by only single user at a time.
- A variable can be borrowed by single user only at a time.
- A data can be altered by a single user which borrow it.
- When variable is borrowed then it cannot be accessible for any other users at that time. It must be released before it is made accessible to others.

```rs
//# Single borrow - Variable assignment
fn main() {
  let mut a = [1, 2, 3];
  // borrow
  let b = &mut a;
  // alter
  b[0] = 4;
  println!("{:?}", b);
  // [4, 2, 3]
}
```

```rs
//# Single borrow - Function parameter
fn main() {
  let mut s1 = String::from("hello");
  // borrow
  foo(&mut s1);
  println!("main says: {}", s1);
}

fn foo (s2: &mut String) {
  s2.push_str(" world!");
  println!("foo says: {}", s2);
}
// foo says: hello world!
// main says: hello world!
```

```rs
//# Multiple borrow - Variable assignment - Gives an error
fn main() {
  let mut s = String::from("hello");
  // Error! cannot borrow `s` as mutable more than once at a time
  let r1 = &mut s;
  let r2 = &mut s;
  println!("{}, {}", r1, r2);
}
```

```rs
//# Multiple borrow - Variable assignment - No error
// Because when variable goes out of scope
// another variable can borrow
fn main() {
  let mut s = String::from("hello");

  {
    let r1 = &mut s;
    println!("{}", r1);
  } // r1 goes out of scope here
  // `s` is released and free so we can
  // borrow again
  let r2 = &mut s;
  println!("{}", r2);
}
// hello
// hello
```

```rs
//# Multiple borrow - Function parameter - No error
// Because after first function execution is over
// then borrow goes out of scope (released) and in next function
// variable can be borrowed again by passing as parameter
fn main() {
  let mut s1 = String::from("hello");
  // borrowed by `foo`
  foo(&mut s1);
  // `s1` is free
  // Can be borrowed again
  bar(&mut s1);
}

fn foo (s2: &mut String) {
  s2.push_str(" foo!");
  println!("foo says: {}", s2);
} // `s2` goes out of scope here
// therefore `s1` is free to borrow again

fn bar (s2: &mut String) {
  s2.push_str(" bar!");
  println!("bar says: {}", s2);
}
// foo says: hello foo!
// bar says: hello foo! bar!
```

> **NOTE:** When variable who has borrowed, goes out of scope then the owner's variable is again available to borrow untill then you cannot borrow again.
