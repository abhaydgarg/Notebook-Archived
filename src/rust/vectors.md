# Vectors

A vector is kind of **a re-sizable array** but **all elements must be in the same type**.

> It’s a generic type, written as `Vec<T>`. `T` can have any type, ex. `Vec<i32>`.

Vectors always allocate their data in a dynamically allocated heap.

```rs
//#1 With new()
let mut a = Vec::new();
let mut a1: Vec<i32> = Vec::new();

//#2 Using the vec! macro
let mut b = vec![];
let mut b1: Vec<i32> = vec![];
let mut b2: Vec<i32> = vec![1, 2, 3];
// Sufixing 1st value with data type
let mut b3 = vec![1i32, 2, 3];

```

## Access

```rs
let mut c = vec![5, 4, 3, 2, 1];
c[0] = 1;
c[1] = 2;
```

## Change data

```rs
// push and pop
let mut d: Vec<i32> = Vec::new();
d.push(1); // [1] : Add an element to the end
d.push(2); // [1, 2]
d.pop(); // [1] : : Remove an element from the end
```

## Capacity and reallocation

Vector represent 3 things:

- A **pointer** to the data
- **No of elements** currently have(**length**)
- **Capacity** (Amount of space allocated for any future elements).

If the length of a vector exceeds its capacity, its capacity will be increased automatically. But its elements will be reallocated(which can be slow). So always use Vec::**with_capacity** whenever it’s possible.

```rs
let mut e: Vec<i32> = Vec::with_capacity(10);
println!("Length: {}, Capacity : {}", e.len(), e.capacity());
// Length: 0, Capacity : 10

// Done without reallocating...
for i in 0..10 {
  e.push(i);
}

// ...but this may make the vector reallocate
// Exceeding capacity
e.push(11);
```

## Iteration

```rs
let mut v = vec![1, 2, 3, 4, 5];

for i in &v {
  println!("A reference to {}", i);
}

for i in &mut v {
  println!("A mutable reference to {}", i);
}

for i in v {
  println!("Take ownership of the vector and its element {}", i);
}
```
