# Slice type

> It does not have ownership. It is always a reference.

Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.

## String slices

A string slice is a reference to part of a String, and it looks like this:

```rs
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

Rather than a reference to the entire `String`, it’s a reference to a portion of the `String`.

We can create slices using a range within brackets by specifying `[starting_index..ending_index]`, where `starting_index` is the first position in the slice and `ending_index` is one more than the last position in the slice. Internally, the slice data structure stores the starting position and the length of the slice, which corresponds to `ending_index` minus `starting_index`. So in the case of `let world = &s[6..11];`, `world` would be a slice that contains a pointer to the 7th byte of `s` with a length value of 5.

![String slice referring to part of a String](assets/trpl04-06.svg)

```rs
// if you want to start at the first index (zero),
// you can drop the value before the two periods.
// In other words, these are equal
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];

// By the same token, if your slice includes the
// last byte of the String, you can drop the trailing number.
// That means these are equal
let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..];

// You can also drop both values to take a slice
// of the entire string. So these are equal
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

### Problem

```rs
// Return the index of the end of the word
fn first_word(s: &String) -> usize {
  let bytes = s.as_bytes();
  for (i, &item) in bytes.iter().enumerate() {
    if item == b' ' {
      return i;
    }
  }
  s.len()
}

fn main() {
  let mut s = String::from("hello world");
  let word = first_word(&s); // word will get the value 5
  s.clear(); // this empties the String, making it equal to ""

  // word still has the value 5 here, but there's no more string that
  // we could meaningfully use the value 5 with. word is now totally invalid!
}
```

**This program compiles without any errors** and would also do so if we used `word` after calling `s.clear()`. Because `word` isn’t connected to the state of `s` at all, `word` still contains the value `5`. We could use that value `5` with the variable `s` to try to extract the first word out, but this would be a bug because the contents of `s` have changed since we saved `5` in `word`.

Luckily, Rust has a solution to this problem: string slices.

Using string slices, we let compiler ensure the references into the `String` remain valid.

```rs
// We get the index for the end of the word in the same way as we did above,
// by looking for the first occurrence of a space. When we find a space,
// we return a string slice using the start of the string and the index
// of the space as the starting and ending indices
fn first_word(s: &String) -> &str {
  let bytes = s.as_bytes();
  for (i, &item) in bytes.iter().enumerate() {
    if item == b' ' {
      return &s[0..i];
    }
  }

  &s[..]
}
// Now when we call first_word, we get back a single value
// that is tied to the underlying data. The value is made up
// of a reference to the starting point of the slice and
// the number of elements in the slice
fn main() {
  let mut s = String::from("hello world");
  let word = first_word(&s);
  // cannot borrow `s` as mutable because it is also borrowed as immutable
  s.clear(); // error!
  println!("the first word is: {}", word);
}
// Recall from the borrowing rules that if we have an
// immutable reference to something, we cannot also take a
// mutable reference. Because `clear` needs to truncate the `String`,
// it tries to take a mutable reference, which fails
```
