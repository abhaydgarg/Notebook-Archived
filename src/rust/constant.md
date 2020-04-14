# Constants

- Rust has two different types of constants `const` and `static`.
- Can be declared in any scope including **global**.
- Require explicit type annotation.
- `const` and `static` should always be in **SCREAMING_SNAKE_CASE**.

```rs
// Globals are declared outside all other scopes.
static LANGUAGE: &str = "Rust";
const THRESHOLD: i32 = 10;

fn main() {
  // Access constant in the main thread
  println!("This is {}", LANGUAGE);
  println!("The threshold is {}", THRESHOLD);

  // Error! Cannot modify a `const`.
  THRESHOLD = 5;
}
```

## const vs static

- `const` is always immutable. You neither can reassign nor modify it. Whereas `static` can be mutable and therefore can either be modified or reassigned.
- `const` are inlined at compilation, which means they're copied to every location they're used, and thus are usually more efficient. Whereas `static` refers to a unique location in memory and are more like global variables.

```rs
//# const

// Create memory location. Say M_0
const THRESHOLD: i32 = 10;

fn main() {
  // Create new memory location
  // and copy THRESHOLD value
  // and then use it. Say M_1
  println!("The threshold is {}", THRESHOLD);

  // Create new memory location again
  // and copy THRESHOLD value
  // and then use it. Say M_2
  println!("The threshold is {}", THRESHOLD);

  // Create new memory location again
  // and copy THRESHOLD value
  // and then use it. Say M_3
  println!("The threshold is {}", THRESHOLD);
}

//# static

// Create memory location. Say M_0
static THRESHOLD: i32 = 10;

fn main() {
  // Refer to location M_0.
  println!("The threshold is {}", THRESHOLD);

  // Refer to location M_0.
  println!("The threshold is {}", THRESHOLD);

  // Refer to location M_0.
  println!("The threshold is {}", THRESHOLD);
}
```
