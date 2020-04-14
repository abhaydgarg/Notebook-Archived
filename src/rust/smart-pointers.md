# Smart pointers

A pointer is a general concept for a variable that contains an address in memory. This address refers to, or “points at,” some other data. The most common kind of pointer in Rust is a reference. References are indicated by the `&` symbol and borrow the value they point to. They don’t have any special capabilities other than referring to data.

Smart pointers, on the other hand, are data structures that not only act like a pointer but also have additional metadata and capabilities. The concept of smart pointers isn’t unique to Rust: smart pointers originated in C++ and exist in other languages as well. In Rust, the different smart pointers defined in the standard library provide functionality beyond that provided by references. One example that we’ll explore in this chapter is the reference counting smart pointer type. This pointer enables you to have multiple owners of data by keeping track of the number of owners and, when no owners remain, cleaning up the data.

In Rust, which uses the concept of ownership and borrowing, an additional difference between references and smart pointers is that **references are pointers that only borrow data**; in contrast, **in many cases, smart pointers own the data they point to**.

`String` and `Vec<T>` count as smart pointers because they own some memory and allow you to manipulate it. They also have metadata (such as their capacity) and extra capabilities or guarantees (such as with String ensuring its data will always be valid UTF-8).
