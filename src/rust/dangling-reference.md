# Dangling reference

> Dangling references are pointers to data that has been deallocated.

Let’s try to create a dangling reference, which Rust will prevent with a compile-time error:

```rs
fn main() {
  let reference_to_nothing = foo();
}

fn foo() -> &String {
  let s = String::from("hello");
  &s
} // Here, `s` goes out of scope, and is dropped.
// Its memory goes away. Danger!
```

Because `s` is created inside `foo()`, when the code of `foo()` is finished, `s` will be deallocated. But we tried to return a reference to it. That means this reference would be pointing to an invalid String. That’s no good! Rust won’t let us do this.

The solution here is to return the String directly:

```rs
fn no_dangle() -> String {
  let s = String::from("hello");
  s
}
// This works without any problems.
// Ownership is moved out, and nothing is deallocated.
```
