# Error and None propagation

We should use panics like `panic!()`, `unwrap()`, `expect()` only if we can not handle the situation in a better way.

If a function contains expressions which can produce either `None` or `Err`,

- we can handle them inside the same function. Or,
- we can return  `None`  and  `Err`  types immediately to the caller. So the caller can decide how to handle them.

> `None` types no need to handle by the caller of the function always. But Rusts’ convention to handle **`Err`** types is, **return them immediately to the caller to give more control to the caller to decide how to handle them.**

## ?

- If an  `Option`  type has  **`Some`**  value or a  `Result`  type has a  **`Ok`**  value,  **the value inside them**passes to the next step.
- If the  `Option`  type has  **`None`**  value or the  `Result`  type has  **`Err`**  value,  **return them immediately**to the caller of the function.

**Option** type

```rs
fn main() {
  if complex_function().is_none() {
    println!("X not exists!");
  }
}

fn complex_function() -> Option<&'static str> {
  // if None, returns immidiately; if Some("abc"), set x to "abc"
  let x = get_an_optional_value()?;
  // some other code, ex
  println!("{}", x);
  // "abc" ; if you change line 19 `false` to `true`

  Some("")
}

fn get_an_optional_value() -> Option<&'static str> {
  //if the optional value is not empty
  if false {
    return Some("abc");
  }
  //else
  None
}
```

**Result** type

```rs
fn main() {
  // `main` function is the caller of `complex_function` function
  // So we handle errors of complex_function(), inside main()
  if complex_function().is_err() {
    println!("Can not calculate X!");
  }
}

fn complex_function() -> Result<u64, String> {
  // if Err, returns immidiately; if Ok(255), set x to 255
  let x = function_with_error()?;
  // some other code, ex
  println!("{}", x);
  // 255 ; if you change line 20 `true` to `false`

  Ok(0)
}

fn function_with_error() -> Result<u64, String> {
  //if error happens
  if true {
    return Err("some message".to_string());
  }
  // else, return valid output
  Ok(255)
}
```

## try!()

> `?` operator was added in Rust version 1.13. `try!()` macro is the old way to propagate errors before that. So we **should avoid** using this now.
