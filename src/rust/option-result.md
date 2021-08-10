# Option and result

Many languages use **`null`, `nil`, `undefined` types** to represent empty outputs, and **`Exceptions`** to handle errors. Rust skips using both, especially to prevent issues like **null pointer exceptions, sensitive data leakages through exceptions** and etc. Instead, Rust provides two special **generic enums**;`Option` and `Result` to deal with above cases.

## Option

> An **Option** can have either **Some** value or **None** (no value).

```rs
// T is a generic and it can contain any type of value.
enum Option<T> {
  Some(T),
  None,
}
```

### Optional return

In a function below outputs a `&str` value and the output can be empty, the return type of the function should set as `Option<&str>`.

```rs
fn get_an_optional_value() -> Option<&str> {

  //if the optional value is not empty
  return Some("Some value");

  //else
  None
}
```

### Optional property

Same way, if the value of a property of a data type can be empty or optional like the `middle_name` of `Name` data type in the following example, we should set its data type as an `Option` type.

```rs
struct Name {
  first_name: String,
  middle_name: Option<String>, // middle_name can be empty
  last_name: String,
}
```

### Using `match` to check type of return value

We can use pattern matching to catch the relevant return type (`Some`/ `None`) via `match`.

```rs
use std::env;

fn main() {
  let home_path = env::home_dir();
  match home_path {
    // This prints home directory path,
    Some(p) => println!("{:?}", p),
    None => println!("Can not find the home directory!"),
  }
}
```

### Optional argument in function

```rs
// middle name can be empty
fn get_full_name(fname: &str, lname: &str, mname: Option<&str>) -> String {
  match mname {
    Some(n) => format!("{} {} {}", fname, n, lname),
    None => format!("{} {}", fname, lname),
  }
}

// However, when using optional arguments with functions,
// we have to pass `None` values for empty arguments
// while calling the function.
fn main() {
  println!("{}", get_full_name("Galileo", "Galilei", None));
  println!("{}", get_full_name("Leonardo", "Vinci", Some("Da")));
}
```

## Result

> A **result** can represent either **Ok** (success) or **Err** (failure).

```rs
// T and E are generics. T can contain any type
// of value, E can be any error.
enum Result<T, E> {
  Ok(T),
  Err(E),
}
```

If a function can produce an error, we have to use a `Result` type by **combining the data type of the valid output and the data type of the error**.

For example, if the data type of the valid output is `u64` and error type is `String`, return type should be `Result<u64, String>`.

```rs
fn function_with_error() -> Result<u64, String> {
  //if error happens
  return Err("The error message".to_string());
  // else, return valid output
  Ok(255)
}
```

We can use the pattern matching to catch the relevant return types (`Ok/Err`) via `match`.

There is a function to fetch the value of any environment variable in **`std::env`** as **[`var()`](https://doc.rust-lang.org/std/env/fn.var.html)** . Its input is the environment variable name. This can produce an error, if we passes a wrong environment variable or the program can not extract the value of the environment variable while running. So, its return type is a `Result` type; [`Result<String, VarError>`](https://doc.rust-lang.org/std/env/enum.VarError.html).

```rs
use std::env;

fn main() {
  let key = "HOME";
  match env::var(key) {
    // This prints "/root", if you run this in Rust playground
    Ok(v) => println!("{}", v),
    // This prints "environment variable not found",
    // if you give a nonexistent environment variable
    Err(e) => println!("{}", e),
  }
}
```

## is_some(), is_none(), is_ok(), is_err()

Other than `match` expressions, Rust provides `is_some()` , `is_none()` and `is_ok()` , `is_err()`functions to identify the return type.

```rs
fn main() {
  let x: Option<&str> = Some("Hello, world!");
  assert_eq!(x.is_some(), true);
  assert_eq!(x.is_none(), false);

  let y: Result<i8, &str> = Ok(10);
  assert_eq!(y.is_ok(), true);
  assert_eq!(y.is_err(), false);
}
```

## ok(), err()

Rust provides `ok()` and `err()` for `Result` types. They convert the `Ok<T>` and `Err<E>` values of a **`Result` type to `Option` types**.

```rs
fn main() {
  let o: Result<i8, &str> = Ok(8);
  let e: Result<i8, &str> = Err("message");

  assert_eq!(o.ok(), Some(8)); // Ok(v) ok = Some(v)
  assert_eq!(e.ok(), None);    // Err(v) ok = None

  assert_eq!(o.err(), None);            // Ok(v) err = None
  assert_eq!(e.err(), Some("message")); // Err(v) err = Some(v)
}
```
