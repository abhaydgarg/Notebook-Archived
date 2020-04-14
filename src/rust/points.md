# Points

## struct property that is movable type

```rs
fn main() {

  let s = S::new(String::from("abc"), 123);
  // `get_x` uses `self` as arg which means
  // the ownership of `s` is moved to
  // method `get_x()` and now new owner is
  // `self` of mehtod `get_x()`. Remember, struct is
  // movable type not copy type.
  let x = s.get_x();
  // You get value here.
  println!("{:?}", x); // abc
  // Error: because you are using `s`
  // which is moved and `s` is no longer
  // a owner of struct.
  println!("{:?}", s);

}

#[derive(Debug)]
struct S {
  x: String,
  y: u32,
}

impl S {
  fn new(x: String, y: u32) -> S {
    S {
      x,
      y,
    }
  }

  // `self` is now owner of
  // `s`.
  fn get_x(self) -> String {
    self.x
  }

  fn get_y(&self) -> u32 {
    self.y
  }

}
```

```rs
fn main() {

  let s = S::new(String::from("abc"), 123);
  // `get_x` uses `&self` as arg which means
  // the ownership of `s` is not moved to
  // method `get_x()` but `get_x()` method borrow
  // `s`.
  let x = s.get_x();
  let y = s.get_y();

  println!("{:?}", x);
  println!("{:?}", y);
  println!("{:?}", s);

}

#[derive(Debug)]
struct S {
  x: String,
  y: u32,
}

impl S {
  fn new(x: String, y: u32) -> S {
    S {
      x,
      y,
    }
  }

  fn get_x(&self) -> String {
    // Error: cannot move out of borrowed content.
    // `s` is borrowed by this method which is a reference
    // and `x` is of type `String` which is a movable type
    // so returning `self.x` try to move ownership of `x` to
    // implicit return variable, which is not possible because Rust
    // does not allow moving ownership if struct borrowed by
    // using `&self`. It works if you use `self` as arg then
    // self will be owner and you can move the ownership of `x`.
    self.x
  }

  // To make method `get_x` works, you must return the
  // reference of value `x`, which does not move ownership.
  // Like so:
  fn get_x(&self) -> &String {
    &self.x
  }

  // To make `get_x` work without brrowing the
  // `s` using arg `&self`. You can write the mehtod like:
  fn get_x(self) -> String {
    // You make a copy of `x` property
    // and return. The ownership does not move.
    let a = self.x.clone();
    a
  }

  // Interstingly, this works fine, even
  // though we are not returning reference
  // of `y` as in `get_x` method. This is because
  // `y` is a copy type, so Rust make a copy of the
  // `y` then return it. Ownership of `y` is not being moved
  // here to implicit returned variable but a copy.
  fn get_y(&self) -> u32 {
    self.y
  }

}
```

```rs
fn main() {

  let s = S::new(String::from("abc"), 123);

  let x = s.get_x();

  println!("{:?}", x);

}

#[derive(Debug)]
struct S {
  x: String,
  y: u32,
}

impl S {
  fn new(x: String, y: u32) -> S {
    S {
      x,
      y,
    }
  }

  // `self` is now owner of
  // `s`.
  fn get_x(self) -> String {
    // Moved the ownership of
    // `self.x` to `a`. Now, struct `S`
    // property `x` is no longer a owner but
    // new owner is `a`. Because `x` property is
    // movable type.
    let a = self.x;
    // Error: value borrowed here after partial move.
    // You are trying to use `self` which is borrowed by
    // `println!` function but Rust sees that the property
    // `x` of `self` is no longer a owner so it does not allow
    // borrowing of `self` unless the ownership is returned to `x`.
    println!("{:?}", self);
    a
  }

  // To make `get_x` method work, you can rewrite it like:
  fn get_x(self) -> String {
    let a = self.x.clone();
    println!("{:?}", self);
    a
  }

  fn get_y(&self) -> u32 {
    self.y
  }

}
```
