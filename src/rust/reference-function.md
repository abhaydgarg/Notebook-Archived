# Reference in function parameter

## & in function signature

When function's signature has a reference (`&`) mentioned then you have to provide a reference to the function at the time of calling.

### Pass non-reference variable when calling function

```rs
fn main() {
  let a = 1;
  // You have to append `a`
  // with `&` because funciton `f`
  // wants reference which points to the
  // value having type `u32`. See the
  // function signature.
  f(a); // Error
}

fn f(arg: &u32) {
  println!("{}", arg);
}
```

#### Correct way

```rs
fn main() {
  let a = 1;
  // function `f` wants parameter which is
  // a reference and value it points to must have
  // type `u32`.
  f(&a); // 1
}

fn f(arg: &u32) {
  println!("{}", arg);
}
```

### Pass reference variable when calling function

```rs
fn main() {
  let a = 1;
  let b = &a;
  // Since `b` is a reference because
  // it contains the reference of `a`, so
  // when calling the function `f`, you do not
  // need to append `&`.
  f(b);
}

fn f(arg: &u32) {
  println!("{}", arg);
}
```

#### What if you append `&`

It works with and without `&`.

> **TODO:** Why does it work with and without `&`?

**My explanation:** When calling `f(&b)`, compiler sees that you have provided a reference which function signature wants, so first check is passed. In second check when compiler check the value's type that reference points to, which must be of type `u32` then on checking `arg` value, it first gets the memory location of `b`, then on checking the value stored in `b`, it gets a memory location of `a`, then on chekcing the value stored in `a`, it gets a value not a refernce or memory location which is a `1` and then at the end copiler check the type of `1` which trun out to be `u32` as expected as per `f` signature.

```rs
fn main() {
  let a = 1;
  let b = &a;
  // See in above example, we do not
  // append `&` because `b` is already a
  // reference. But it works also with
  // `&`.
  f(&b);
}

fn f(arg: &u32) {
  println!("{}", arg);
}
```
