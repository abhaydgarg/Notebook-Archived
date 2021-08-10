# Compound data types

> Contain multiple values.

## array

> **Fixed size** list of elements of **same data type**.

Arrays are immutable by default and even with `mut`, its **element count cannot be changed**.

```rs
// Empty array
let a = [];

// Regular annotation
let a = [1, 2, 3];

// [Type; NO of elements] annotation
let a: [i32; 3] = [1, 2, 3];
```

### Read array elements

```rs
let a = [1, 2, 3];

a[0] // 1
a[1] // 2
a[2] // 3
```

## tuples

> **Fixed size** ordered list of elements of **different(or same) data types**.

Tuples are immutable by default and even with `mut`, its **element count cannot be changed**.

```rs
let a = (1, 1.5, true, 'a', "Hello, world!");

let a: (i32, f64) = (1, 1.5);
```

### Read tuple elements

```rs
let a = (1, 1.5, true, 'a', "Hello, world!");

a.0 // 1
a.1 // 1.5
a.2 // true
a.3 // 'a'
a.4 // "Hello, world!"

// Destructuring
let a: (i32, f64) = (1, 1.5);
let (b, c) = a; // b = 1, c = 1.5
```
