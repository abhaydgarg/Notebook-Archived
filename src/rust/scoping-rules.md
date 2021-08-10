# Scoping rules

Scopes play an important part in ownership, borrowing, and lifetimes. That is, they indicate to the compiler when borrows are valid, when resources can be freed, and when variables are created or destroyed.

## Memory management

In every application, you must allocate new storage before you can use it, and then you must return all such storage to the operating system when you are finished. While some languages handle all of this for you, lower-level languages like C or C++ sometimes require more effort by the programmer to manage memory. There are several ways to do this: implicit and explicit.

### Implicit

Implicit memory allocation is memory that is allocated, typically on the system stack, by the compiler. This occurs when you create a new variable:

```c
void my_func(void) {
  int x;
  char y;
  int z[25];
  ...
}
```

Here, space is allocated on the system stack for the variables `x`, `y` and `z` when the function is called, and the space is reclaimed automatically when the function exits.

### Explicit

Explicit memory allocation occurs through pointers, and calls to the memory management functions.

There are also many places where memory needs to be created in a size which is not known at compile-time. Consider a web browser that needs to allocate enough storage to store the downloaded HTML text of a webpage. Since webpages can be all different sizes, we can't possibly know the sizes of them all when we compile, we must allocate the storage at runtime with pointer or an equivalent.

> Memory that is allocated on the stack and is fixed in size and scope is called **static**. Memory that uses pointer to be allocated at run time is called **dynamic**.

### Freeing dynamic memory

When you allocate memory from the heap using `malloc` or something similar, you obtain a pointer to allocated memory from the system. When you are done with it, you must free the memory, to return it back to the system. Failure to return all the memory that you allocate is called a **memory leak**.

When you allocate memory, you must always be careful to release your allocated memory back to the system. Herein is the problem: if your system is complicated and needs to allocate many small chunks of memory, you need to be certain to free all those individual little chunks as well. This can be all the more difficult if you are in a dynamic situation where memory is being allocated and deallocated continuously, and memory chunks must stay available for random lengths of time.

To overcome these complications automatic memory manager systems, such as garbage collectors, can be employed to try to automate the memory allocations and deallocations.

## Memory and other programming languages

C and C++ give a total control to programmer to handle memory allocation and deallocation. They do not have garabage collector.

Let's understand the memory allocation and removal through **Javascript language** which uses garabage collector to free unused memory space.

> Most other languages such as Java, PHP etc uses almost the same concept for handling primitive and non-primitive data types.

### Primitive types

- Store in stack (Execution context).
- Destoryed when stack is removed from the stack frame.
- Uses **copy by value** mechanism when assign to variable and pass variable to function as parameter.

```js
// the value of a is copied to b.
// a and b has their own separate copy of
// value 1.
let a = 1;
let b = a;

// reassign a
a = null;
console.log(b) // 1

//# Function parameters
// Copy first then pass
// own separate copy of values
// to function.
function add(a, b) {
  // Changing a or b here
  // do not affect a and b variables
  // outside the function because they
  // have been copied.
  return a + b;
}

let a = 1;
let b = 2;
// a and b passed to function
// are first copied and then passed.
// This is IMPLICITLY or automatically
// done by javascript.
let sum = add(a, b);
```

### Non-primitive types

- Store in heap.
- Reference or pointer which points to the memory location in heap is stored in stack in a variable.
- Pointer is destoryed when stack is removed from the stack frame. Which does not remove memory from heap therefore garabage collector runs time ot time in order to remove unused memory which has no pointer back in stack frame.
- Uses [**by share**](https://abhaydgarg.github.io/Notebook/javascript/js/reference-concept/) mechanism when assign to variable and pass variable to function as parameter.
- By share mechanism because non-primitive types can have large data associated with them so making a multiple copy and store ina heap is a wastage of space and has performance set back because now we have to maintain or keep all the copies in sync.

> Object and Array are non-primitive types in javascript.

```js
// the value of a is actually a pointer
// or address of memory location in heap
// where a is stored. Assign a to b copy the
// memory address to b.
let a = { x: 1, y: 2};
let b = a;

// Changes both a and b
// because both have same
// memory address.
a.x = 2;
console.log(a) // { x: 2, y: 2}
console.log(b) // { x: 2, y: 2}

// reassign a, which does not
// change the memory address stored in
// b because both a and b have their own
// separate copy of memory address.
// This is called `by share` mechanism.
a = null;
console.log(a) // null
console.log(b) // { x: 2, y: 2}

//# Function parameter
function fn(a) {
  // Changing a here do affect a variable
  // outside the function.
  a.x = 2;
}
let a = { x: 1, y: 2};
fn(a);
console.log(a) // { x: 2, y: 2}
```
