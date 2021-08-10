# Cargo and crates

## Cargo

Cargo is Rust’s built-in package manager and the build system. It can be used to,

- Create a new project: `cargo new`
- Build the project: `cargo build`
- Run the project: `cargo run`
- Update project dependencies:  `cargo update`
- Run tests: `cargo test`
- Generate the project documentation via [rustdoc](https://doc.rust-lang.org/stable/rustdoc/):  `cargo doc`
- Analyze the project to see it has any errors, without building it: `cargo check`

In addition, there are `cargo` commands to publish the project as a crate/ package to **Rust’s official crate registry, [crates.io](https://crates.io/)**.

## Crate

A crate is a package, which can be shared via [crates.io](https://crates.io/). A crate can produce an executable or a library. In other words, it can be a **binary** crate or a **library** crate.

### Produces an executable

`cargo new crate_name --bin` or `cargo new crate_name`

```
├── Cargo.toml
└── src
    └── main.rs
```

### Produces a library

`cargo new crate_name --lib`

```
├── Cargo.toml
└── src
    └── lib.rs
```

- **Cargo.toml**(capital c) is the configuration file which contains all of the metadata that Cargo needs to compile your project.
- **src** folder is the place to store the source code.
- Each crate has an implicit crate root/ entry point. **main.rs** is the crate root for a binary crate and **lib.rs** is the crate root for a library crate.
