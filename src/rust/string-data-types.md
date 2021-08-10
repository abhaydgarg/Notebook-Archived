# String data types

## String literal [&str]

> It is an immutable **fixed-length** string somewhere in memory.

Prefer `&str` as a function parameter or if you want a read-only view of a string.

```rs
let a = "Hello, world";
let a: &str = "Hello, world";

// `a` is a variable
// "Hello, world" is a *literal*
```

The type of `a` here is `&str`: it’s a slice pointing to that specific point of the binary. This is also why string literals are immutable; `&str` is an immutable reference.

### Always an immutable reference

No such thing `str`, it is always `&str`.

> String literal is a slice. [More about slice type](rust/slice-type/)

It is always a `reference` and does not have ownership. So when you expilcitly mention type, you always use `&str` not `str`.

```rs
fn main() {
  // `s` has no ownership rather it is
  // reference
  let s = "Hello world!";
  // `s` is already a reference, so no need
  // to use a `&` when calling
  f(s);
  // Hello world!
}

// Since `s` is always a reference so we
// need to mention type by `&str`
fn f(arg: &str) {
  println!("{}", arg);
}
```

## String

> A String is a heap-allocated string. This string is **growable** and is also guaranteed to be UTF-8.

Prefer `String` when you want to own and mutate a string.

```rs
let mut s = String::from("Hello, Rust!");
let mut s: String = String::from("Hello, Rust!");
```

## Which one to use?

Use `String` if you need to own data (like passing strings to other threads, or building them at runtime), and use `&str` if you only need a view of a string and you do not want to own the data.
