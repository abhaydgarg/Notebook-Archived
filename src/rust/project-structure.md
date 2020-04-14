# Project structure

```
├── Cargo.lock
├── Cargo.toml
├── benches
│   └── large-input.rs
├── examples
│   └── simple.rs
├── src
│   ├── bin
│   │   └── another_executable.rs
│   ├── lib.rs
│   └── main.rs
└── tests
    └── some-integration-tests.rs
```

- The source code goes in the `src` directory.
- The default library file is `src/lib.rs`.
- The default executable file is `src/main.rs`.
- Other executables can be placed in `src/bin/*.rs`.
- Integration tests go in the `tests` directory (unit tests go in each file they’re testing).
- Examples go in the `examples` directory.
- Benchmarks go in the `benches` directory.
