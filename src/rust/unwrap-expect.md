# Unwrap and expect

## unwrap()

- If an  `Option`  type has  **`Some`**  value or a  `Result`  type has a  **`Ok`**  value,  **the value inside them** passes to the next step.
- If the  `Option`  type has  **`None`**  value or the  `Result`  type has  **`Err`**  value,  **program panics**; If  `Err`, panics with the error message.

### Without using unwrap

```rs
//# With `Option` and `match`.
fn main() {
  let x;
  match get_an_optional_value() {
    Some(v) => x = v, // if Some("abc"), set x to "abc"
    None => panic!(), // if None, panic without any message
  }

  println!("{}", x); // "abc" ; if you change line 14 `false` to `true`
}

fn get_an_optional_value() -> Option<&'static str> {

  //if the optional value is not empty
  if false {
    return Some("abc");
  }

  //else
  None
}

// --------------- Compile-time error ---------------
// thread 'main' panicked at 'explicit panic', src/main.rs:5:17

//# With `unwrap`.
fn main() {
  let x = get_an_optional_value().unwrap();
  println!("{}", x);
}

// --------------- Compile-time error ---------------
// thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', libcore/option.rs:345:21
```

Example with `Result` and `match`.

```rs
fn main() {
  let x;
  match function_with_error() {
    Ok(v) => x = v, // if Ok(255), set x to 255
    Err(e) => panic!(e), // if Err("some message"), panic with error message "some message"
  }

  println!("{}", x); // 255 ; if you change line 13 `true` to `false`
}

fn function_with_error() -> Result<u64, String> {
  //if error happens
  if true {
    return Err("some message".to_string());
  }

  // else, return valid output
  Ok(255)
}

// ---------- Compile-time error ----------
// thread 'main' panicked at 'some message', src/main.rs:5:19

//# With `unwrap`.
fn main() {
  let x = function_with_error().unwrap();
  println!("{}", x);
}

// --------------- Compile-time error ---------------
// thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: "some message"', libcore/result.rs:945:5
```

> But as you can see, when using `unwrap()` error messages are not showing the exact line numbers where the panic happens.

## unwrap_or()

If an `Option` type has `Some` value or a `Result` type has a `Ok` value, the value inside them passes to the next step.

But when having `None` or `Err` then it does not panic but value provided inside `_or()` parenthesis is passed to next step.

> The data type of value inside `_or()` must match with the data type of the relevant `Some` or `Ok`.

```rs
let v = 8;
let n = None;

// Pass `v` to next step.
n.unwrap_or(v);
```

## unwrap_or_default()

If an `Option` type has `Some` value or a `Result` type has a `Ok` value, the value inside them passes to the next step.

But when having `None` or `Err` then the default value of the data type of the relevant `Some` or `Ok`, is passing to the next step.

```rs
// Converts a numeric string to an integer.

let good_year_from_input = "1909";
let bad_year_from_input = "190blarg";
// Pass 1909 and assign to `good_year`.
let good_year = good_year_from_input.parse().ok().unwrap_or_default();
// Pass 0 and assign to `bad_year`. Because `bad_year_from_input` is a
// non-numeric string which retuns `Err` and 0 is the default value
// for integer, so 0 is passed to next step.
let bad_year = bad_year_from_input.parse().ok().unwrap_or_default();
```

## unwrap_or_else()

Similar to `unwrap_or()`. The only difference is, instead of passing a value, you have to pass a **closure** which returns a value with the same data type of the relevant `Some` or `Ok`.

```rs
let k = 10;
assert_eq!(Some(4).unwrap_or_else(|| 2 * k), 4);
assert_eq!(None.unwrap_or_else(|| 2 * k), 20);
```

## expect()

> Similar to `unwrap()` but can set a custom message for the panics.

```rs
// 01. expect error message for None
fn main() {
  let n: Option<i8> = None;
  n.expect("empty value returned");
}

// --------------- Compile-time error ---------------
// thread 'main' panicked at 'empty value returned', libcore/option.rs:989:5


// 02. expect error message for Err
fn main() {
  let e: Result<i8, &str> = Err("some message");
  e.expect("expect error message");
}

// --------------- Compile-time error ---------------
// thread 'main' panicked at 'expect error message: "some message"', libcore/result.rs:945:5
```

## unwrap_err() and expect_err() - Result type

> The opposite case of `unwrap()` and `expect()`; Panics with `Ok` values.

```rs
// 01. unwrap_err error message for Ok
fn main() {
  let o: Result<i8, &str> = Ok(8);
  o.unwrap_err();
}

// ---------- Compile-time error ----------
// thread 'main' panicked at 'called `Result::unwrap_err()` on an `Ok` value: 8', libcore/result.rs:945:5


// 02. expect_err error message for Ok
fn main() {
  let o: Result<i8, &str> = Ok(8);
  o.expect_err("Should not get Ok value");
}

// ---------- Compile-time error ----------
// thread 'main' panicked at 'Should not get Ok value: 8', libcore/result.rs:945:5
```
