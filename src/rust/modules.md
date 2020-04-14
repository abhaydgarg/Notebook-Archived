# Modules

## In the same file

### By default - Everything inside module is private

```rs
//# main.rs
fn main() {
   greetings::hello();
}

mod greetings {
  // function has to be public to access from outside
  pub fn hello() {
    println!("Hello, world!");
  }
}
```

### Nested module

```rs
//# main.rs
fn main() {
  phrases::greetings::hello();
}

mod phrases {
  pub mod greetings {
    pub fn hello() {
      println!("Hello, world!");
    }
  }
}
```

### Call private function

> The `self` keyword is used to refer the same module, while the `super` keyword is used to refer parent module.

```rs
// 01. Calling private functions of the same module
//# main.rs
fn main() {
  phrases::greet();
}

mod phrases {
  pub fn greet() {
    hello(); // Or `self::hello();`
  }

  fn hello() {
    println!("Hello, world!");
  }
}

// 02. Calling private functions of the parent module
//# main.rs
fn main() {
  phrases::greetings::hello();
}

mod phrases {
  fn private_fn() {
    println!("Hello, world!");
  }

  pub mod greetings {
    pub fn hello() {
      super::private_fn();
    }
  }
}
```

### `super` to call function outside module

```rs
//# main.rs
fn main() {
  greetings::hello();
}

fn hello() {
  println!("Hello, world!");
}

mod greetings {
  pub fn hello() {
    super::hello();
  }
}
```

## In a different file, same directory

### No need to wrap the code with a `mod` declaration

> The file itself acts as a module.

```rs
//# main.rs
mod greetings; // Import greetings module

fn main() {
  greetings::hello();
}

//# greetings.rs
pub fn hello() {
  println!("Hello, world!");
}
```

### If we wrap file content with a `mod` declaration, it will act as a nested module

```rs
//# main.rs
mod phrases;

fn main() {
  phrases::greetings::hello();
}

//# phrases.rs
// The module has to be public to access from outside
pub mod greetings {
  pub fn hello() {
    println!("Hello, world!");
  }
}
```

## In a different file, different directory

`mod.rs` is like `index.js` which is by default pick by Rust, inside the folder which act as entry point for module.

> Folder name is a module name.

```rs
//# main.rs
mod greetings;

fn main() {
  greetings::hello();
}

//# greetings/mod.rs
// where `greetings` is a folder name
pub fn hello() {
  println!("Hello, world!");
}
```

### If we wrap file content with a `mod` declaration, it will act as a nested module

```rs
//# main.rs
mod phrases;

fn main() {
  phrases::greetings::hello();
}

//# phrases/mod.rs
// The module has to be public to access from outside
pub mod greetings {
  pub fn hello() {
    println!("Hello, world!");
  }
}
```

### Other files in the directory module act as sub-modules for `mod.rs`

```rs
//# main.rs
mod phrases;

fn main() {
  phrases::hello()
}

//# phrases/mod.rs
mod greetings;

pub fn hello() {
  greetings::hello()
}

//# phrases/greetings.rs
pub fn hello() {
  println!("Hello, world!");
}
```

If you need to access an element of `phrases/greetings.rs` from outside the module, you have to import the `greetings` module as a `public` module.

```rs
//# main.rs
mod phrases;

fn main() {
  phrases::greetings::hello();
}

//# phrases/mod.rs
pub mod greetings; // `pub mod` instead `mod`

//# phrases/greetings.rs
pub fn hello() {
  println!("Hello, world!");
}
```

**Re-export** to call function like this `phrases::hello()` (without `greetings`) istead of `phrases::greetings::hello()`, as in above example:

```rs
//# phrases/greetings.rs
pub fn hello() {
  println!("Hello, world!");
}

//# phrases/mod.rs
pub mod greetings;

// Re-export `greetings::hello` to phrases
pub use self::greetings::hello;

//# main.rs
mod phrases;

fn main() {
  // You can call `hello()` directly from phrases
  phrases::hello();
}
```
