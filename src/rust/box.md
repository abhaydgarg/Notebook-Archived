# `Box<T>` to point data on the heap

> Single ownership.

The most straightforward smart pointer is a _box_, whose type is written `Box<T>`. Boxes allow you to store data on the heap rather than the stack. What remains on the stack is the pointer to the heap data.

`Box<T>` owns the data it points to. We call it smart because when it goes out of scope, it will first drop the data it points to and then itself. No manual memory management required.

![Box](assets/box.png)

Boxes don’t have performance overhead, other than storing their data on the heap instead of on the stack. But they don’t have many extra capabilities either. You’ll use them most often in these situations:

- When you have a type whose size can’t be known at compile time and you want to use a value of that type in a context that requires an exact size
- When you have a large amount of data and you want to transfer ownership but ensure the data won’t be copied when you do so
- When you want to own a value and you care only that it’s a type that implements a particular trait rather than being of a specific type

## Usage

```rs
fn main() {
  let b = Box::new(5);
  println!("b = {}", b);
}
```

We define the variable `b` to have the value of a `Box` that points to the value `5`, which is allocated on the heap. This program will print `b = 5`; in this case, we can access the data in the box similar to how we would if this data were on the stack. Just like any owned value, when a box goes out of scope, as `b` does at the end of `main`, it will be deallocated. The deallocation happens for the box (stored on the stack) and the data it points to (stored on the heap).

## Recursive types with boxes

> When you have a type whose size can’t be known at compile time.

> **Rust need size to be known before hand when storing data in stack, for storing data in heap it does not need to know the size before hand.**

**At compile time, Rust needs to know how much space a type takes up.** One type whose size can’t be known at compile time is a recursive type, where a value can have as part of itself another value of the same type. Because this nesting of values could theoretically continue infinitely, Rust doesn’t know how much space a value of a recursive type needs. However, **boxes have a known size** because it is a pointer which need fixed size in stack to store memory address, so by inserting a box in a recursive type definition, you can have recursive types.

### How Rust compute size of non recursive type

```rs
let a : i32 = 111;
```

To determine how much space to allocate for `a` value, Rust check the type of `a` which is `i32`. Now Rust knows how much space is required to allocate for `a` becuase it knows how much space `i32` type is used (`-(2ⁿ⁻¹) to 2ⁿ⁻¹-1`).

Let's check example below, a binary tree representation.

```rs
// `Tree` is a custom type which is recursive
struct Tree {
  data: i64,
  left: Tree,
  right: Tree,
}
// Error - recursive type `Tree` has infinite size
```

A tree is a structure containing an `i64`, and two trees. Each of these trees is a structure containing an `i64`, and two trees. Each of these...so on. You got an idea. Since we don't know how many subtrees our tree will have, there is no way to tell how much memory we need to allocate up front. We'll only know at runtime.

Rust tells us how to fix that: by using `Box<T>`. Because a `Box<T>` is a pointer, Rust always knows how much space a `Box<T>` needs: a pointer’s size doesn’t change based on the amount of data it’s pointing to. See `Box<T>` is a pointer which contains the memory address, and value of memory address is of fixed size because it is just an address. So Rust store the pointer in stack, so in this way compiler knows the size of `Tree` type before hand which is combination of size of `data: i64 + left: Tree (a pointer size) + right: Tree (a pointer size)`. As you know compiler need to know about the size of variable before hand when it is stored in stack. The actual data is stored in a heap which is allocated at runtime because we cannot determine the size of data at compile time.

We now know that any `Tree` value will take up the size of an `i64` plus the size of a two box’s pointer data. By using a box, we’ve broken the infinite, recursive chain, so the compiler can figure out the size it needs to store a `Tree` value.

> Boxes provide only the indirection and heap allocation; they don’t have any other special capabilities, like with the other smart pointer types. They also don’t have any performance overhead that these special capabilities incur, so they can be useful in cases where the indirection is the only feature we need.

## Indirection

In computer programming, indirection (also called dereferencing) is the ability to reference something using a name, reference, or container instead of the value itself. The most common form of indirection is the act of manipulating a value through its memory address. For example, accessing a variable through the use of a pointer.
