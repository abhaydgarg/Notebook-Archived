# Dereference

A regular reference is a type of pointer, and one way to think of a pointer is as an arrow to a value stored somewhere else. In a code below, we create a reference to an `i32` value and then use the dereference operator to follow the reference to the data:

```rs
fn main() {
  let x = 5;
  let y = &x;

  assert_eq!(5, x);
  assert_eq!(5, *y);
}
```

The variable `x` holds an `i32` value, `5`. We set `y` equal to a reference to `x`. We can assert that `x` is equal to `5`. However, if we want to make an assertion about the value in `y`, we have to use `*y` to follow the reference to the value it’s pointing to (hence _dereference_). Once we dereference `y`, we have access to the integer value `y` is pointing to that we can compare with `5`.

If we tried to write `assert_eq!(5, y);` instead, we would get this compilation error:

```
error[E0277]: can't compare `{integer}` with `&{integer}`
```

> Comparing a number and a reference to a number isn’t allowed because they’re different types. We must use the dereference operator to follow the reference to the value it’s pointing to.
