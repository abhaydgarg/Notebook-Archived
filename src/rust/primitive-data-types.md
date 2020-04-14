# Primitive data types (Scalar)

Rust is a **statically typed** language; it checks data types at compile-time. But it **doesn’t require you to actually type it when declaring variable bindings**. In that case, the compiler checks the usage and sets a better data type for it.

A scalar type represents a `single` value. Rust has `four` primary scalar types: integers, floating-point numbers, Booleans, and characters.

## Integers

> Default = `i32`

### Signed - i8, i16, i32, i64, i128

The min and max values calculated from `-(2ⁿ⁻¹) to 2ⁿ⁻¹-1`

You can use `min_value()` and `max_value()` functions to find min and max of each integer type `i8::min_value();`

| Type | Min                                      | Max                                     |
|------|------------------------------------------|-----------------------------------------|
| i8   | -128                                     | 127                                     |
| i16  | -32768                                   | 32767                                   |
| i32  | -2147483648                              | 2147483647                              |
| i64  | -9223372036854775808                     | 9223372036854775807                     |
| i128 | -170141183460469231731687303715884105728 | 170141183460469231731687303715884105727 |

```rs
// Default = i32
let default_integer = 7;

// Regular annotation
let regular_integer: i64 = 123;

// Suffix annotation
let suffix_integer = 5i32;
```

### Unsigned - u8, u16, u32, u64, u128

The min and max values calculated from `0 to 2ⁿ-1`

You can use `min_value()` and `max_value()` functions to find min and max of each integer type `u8::min_value();`

| Type | Min | Max                                     |
|------|-----|-----------------------------------------|
| u8   | 0   | 255                                     |
| u16  | 0   | 65535                                   |
| u32  | 0   | 4294967295                              |
| u64  | 0   | 18446744073709551615                    |
| u128 | 0   | 340282366920938463463374607431768211455 |

```rs
// Regular annotation
let regular_integer: u64 = 123;

// Suffix annotation
let suffix_integer = 5u32;
```

### isize, usize

Types depend on the kind of computer your program is running on: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

## Floating-point - f32, f64

> Default = `f64`

Rust follows IEEE Standard for Binary Floating-Point Arithmetic.

The `f32` type is similar to **float(Single precision)** in other languages, while `f64` is similar to **double(Double precision)** in other languages.

> Should avoid using `f32`, unless you need to reduce memory consumption badly or if you are doing low-level optimization, when targeted hardware does not support for double-precision or when single-precision is faster than double-precision on it.

```rs
// Default = f64
let a_float = 7.2;

// Regular annotation
let regular_float: f64 = 1.0;

// Suffix annotation
let suffix_float = 5.2f64;
```

## char

A single Unicode scalar value **(4 bytes)**

```rs
let x = 'x';

// no "x", only single quotes
```

## bool

```rs
let x = true;
let y: bool = false;

// no TRUE, FALSE (capital)
```

## unit type `()`

Only possible value is an empty tuple: `()`
