# Use

## Full path into new name

Mainly `use` keyword is used to bind a full path of an element to a new name. So the user doesn’t want to repeat the full path each time.

> By default, `use` declarations use absolute paths, starting from the crate root. But `self` and `super` declarations make that path relative to the current module.

```rs
// -- Initial code without the `use` keyword --
mod phrases {
  pub mod greetings {
    pub fn hello() {
      println!("Hello, world!");
    }
  }
}

fn main() {
  phrases::greetings::hello(); // Using full path
}


// -- Usage of the `use` keyword --
// 01. Create an alias for module
use phrases::greetings;
fn main() {
  greetings::hello();
}

// 02. Create an alias for module elements
use phrases::greetings::hello;
fn main() {
  hello();
}

// 03. Customize names with the `as` keyword
use phrases::greetings::hello as greet;
fn main() {
  greet();
}
```

## Rust standard library

We don’t need to use `extern crate std`; when using the std library.

```rs
// -- 01. Importing elements --
use std::fs::File;

fn main() {
  File::create("empty.txt").expect("Can not create the file!");
}

// -- 02. Importing module and elements --
std::fs::{self, File} // `use std::fs; use std::fs::File;`

fn main() {
  fs::create_dir("some_dir").expect("Can not create the directry!");
  File::create("some_dir/empty.txt").expect("Can not create the file!");
}

// -- 03. Importing multiple elements --
use std::fs::File;
use std::io::{BufReader, BufRead}; // `use std::io::BufReader; use std::io::BufRead;`

fn main() {
  let file = File::open("src/hello.txt").expect("file not found");
  let buf_reader = BufReader::new(file);

  for line in buf_reader.lines() {
    println!("{}", line.unwrap());
  }
}
```

## Re-exporting

Another special case is `pub use`. When creating a module, you can export things from another module into your module. So after that, they can be accessed directly from your module. This is called re-exporting.

```rs
//# main.rs
mod phrases;

fn main() {
  phrases::hello(); // Not directly map
}

//# phrases/mod.rs
pub mod greetings;

// Re-export `greetings::hello` to phrases
pub use self::greetings::hello;

//# phrases/greetings.rs
pub fn hello() {
  println!("Hello, world!");
}
```

This pattern is quite common in large libraries. It helps to hide the complexity of the internal module structure of the library from users. Because users don’t need to know/follow the whole directory map of the elements of the library while working with them.

## External crate

Since Rust 1.0, 99% of all users will use Cargo to manage the dependencies of a project.

```
[dependencies]
old-http = "0.1.0-pre"
```

### Rust 2015

```rs
extern crate old_http;
use old_http::SomeType;
```

### Rust 2018

```rs
use old_http::SomeType;
```
