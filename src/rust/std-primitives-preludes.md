# Standard Library, Primitives and Preludes

In Rust, language elements are implemented by not only `std` library crate but also compiler as well. Examples,

- **Primitives:** Defined by the compiler and methods are implemented by `std` library directly on primitives.
- **Standard Macros:** Defined by both compiler and `std`

While primitives are implemented by the compiler, the standard library implements the most useful methods directly on the primitive types. But some rarely useful language elements of some primitives are stored on relevant `std` modules. This is why you can see `char`, `str` and `integer` types on both `primitives` and `std` modules.

## Primitives

```
// Primitives: Defined by the compiler and
// methods are directly implemented by std
bool, char, slice, str

i8, i16, i32, i64, i128, isize
u8, u16, u32, u64, u128, usize

f32, f64

array, tuple

pointer, fn, reference
```

## Standard Macros

```
// Standard Macros also defined by both compiler and std
print, println, eprint, eprintln
format, format_args
write, writeln

concat, stringify

include, include_bytes, include_str

assert, assert_eq, assert_ne
debug_assert, debug_assert_eq, debug_assert_ne

try, panic, compile_error, unreachable, unimplemented

file, line, column, module_path
env, option_env
cfg

vec
```

## Std Modules

```
// std modules
char, str

i8, i16, i32, i64, i128, isize
u8, u16, u32 ,u64, u128, usize
f32, f64
num

vec, slice, hash, heap, collections

string, ascii, fmt

default

marker, clone, convert, cmp, iter

ops, ffi

option, result, panic, error

io
fs, path
mem, thread, sync
process, env
net
time
os

ptr, boxed, borrow, cell, any, rc

prelude
```

## Few important std modules

- `std::io`  - Core  **I/O**  functionality
- `std::fs`  -  **Filesystem**  specific functionality
- `std::path`  -  **Cross-platform path**  specific functionality
- `std::env`  -  **Process’s environment**  related functionality
- `std::mem`  -  **Memory**  related functionality
- `std::net`  -  **TCP/UDP**  communication
- `std::os`  -  **OS**  specific functionality
- `std::thread`  - Native  **threads**  specific functionality
- `std::collections`  - Core  **Collection types**

## Preludes

Even though Rust std contains many modules, by default it doesn’t load each and everything of std library on every rust program. Instead, it loads only the smallest list of things which require for almost every single Rust program. These are called preludes. They import only,

```rs
// Reexported core operators
pub use marker::{Copy, Send, Sized, Sync};
pub use ops::{Drop, Fn, FnMut, FnOnce};

// Reexported functions
pub use mem::drop;

// Reexported types and traits
pub use boxed::Box;
pub use borrow::ToOwned;
pub use clone::Clone;
pub use cmp::{PartialEq, PartialOrd, Eq, Ord};
pub use convert::{AsRef, AsMut, Into, From};
pub use default::Default;
pub use iter::{Iterator, Extend, IntoIterator};
pub use iter::{DoubleEndedIterator, ExactSizeIterator};
pub use option::Option::{self, Some, None};
pub use result::Result::{self, Ok, Err};
pub use slice::SliceConcatExt;
pub use string::{String, ToString};
pub use vec::Vec;
```

So technically, Rust inserts,

- `extern crate std;`  : into the  **crate root of every crate**
- `use std::prelude::v1::*;`  : into  **every module**

So you don’t need to import these each time.
