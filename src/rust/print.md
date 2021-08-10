# Formatted print

Printing is handled by a series of `macros` defined in `std::fmt` some of which include:

- `format!`: write formatted text to `String`.
- `print!`: same as `format!` but the text is printed to the console (io::stdout).
- `println!`: same as `print!` but a newline is appended.
- `eprint!`: same as `format!` but the text is printed to the standard error (io::stderr).
- `eprintln!`: same as `eprint!`but a newline is appended.

1. `{}` is a placeholder.
2. `{:?}` is a debug placeholder.

```rs
//# The format! macro is used to store the formatted string.
let x = format!("{}, {}!", "Hello", "world");
println!("{}", x); // Hello, world!

//# Simple usage
println!("{}, {}!", "Hello", "world"); // Hello, world!

//# Positional arguments
println!("{0}, {1}!", "Hello", "world"); // Hello, world!

//# Named arguments
println!("{greeting}, {name}!", greeting="Hello", name="world"); // Hello, world!

//# Debug placeholder
println!("{:?}", [1,2,3]); // [1, 2, 3]

//# Debug placeholder with line break
println!("{:#?}", [1,2,3]);
/*
    [
        1,
        2,
        3
    ]
*/
```
