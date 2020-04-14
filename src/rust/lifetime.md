# Lifetime

> It is Rust's mechanism to validate references (&).

When reference come into picture in your code, then compiler implicitly mark the lifetime of reference. Most of the time programmer need not to mark explicitly rather compiler does it behind the scene. But in some cases when compiler is unable to figure out the lifetime of reference then it ask programmer to explicitly mark the lifetime of reference.

> Where there is reference or borrow, there is a lifetime.

## Syntax

Lifetime annotations have a slightly unusual syntax: the names of lifetime parameters must start with an apostrophe (`'`) and are usually all lowercase and very short, like generic types.

```rs
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

## Why is it needed?

To enforce memory safety, compiler mark the lifetime of every refrence in a code to make sure that **any borrow or reference must last no greater than that of the owner.** In other words, compiler shouts an error if it sees that owner is removed from the memory but its reference is still being used. Compiler try to prevent the **Dangling reference** by checking and validating the lifetime of every reference or borrow in a code.

> Rust enforces [the borrowing rules] through lifetimes. Lifetimes are effectively just names for scopes somewhere in the program. Each reference, and anything that contains a reference, is tagged with a lifetime specifying the scope it's valid for.

```rs
{
  let r;

  {
    let x = 5;
    r = &x;
  } // Error -`x` does not live long enough

  println!("r: {}", r);
}
```

The outer scope declares a variable named `r` with no initial value, and the inner scope declares a variable named `x` with the initial value of 5. Inside the inner scope, we attempt to set the value of `r`as a reference to `x`. Then the inner scope ends, and we attempt to print the value in `r`. This code won’t compile because the value `r` is referring to has gone out of scope before we try to use it.

## Borrow checker

> The Rust compiler has a borrow checker that compares scopes to determine whether all borrows are valid.

```rs
{
    let r;                // ---------+-- 'a
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |
        r = &x;           //  |       |
    }                     // -+       |
                          //          |
    println!("r: {}", r); //          |
}
```

Here, we’ve annotated the lifetime of `r` with `'a` and the lifetime of `x` with `'b`. As you can see, the inner `'b` block is much smaller than the outer `'a` lifetime block. At compile time, Rust compares the size of the two lifetimes and sees that `r` has a lifetime of `'a` but that it refers to memory with a lifetime of `'b`. The program is rejected because `'b` is shorter than `'a`: the subject of the reference doesn’t live as long as the reference.

```rs
{
    let x = 5;            // ----------+-- 'b
                          //           |
    let r = &x;           // --+-- 'a  |
                          //   |       |
    println!("r: {}", r); //   |       |
                          // --+       |
}
```

Here, `x` has the lifetime `'b`, which in this case is larger than `'a`. This means `r` can reference `x`because Rust knows that the reference in `r` will always be valid while `x` is valid.

## Problems when programmer has to annotate lifetime expilictly

> 99% of times compiler implicitly annotate the lifetime of references and check them by borrow checker. But in some cases compiler cannot infer or figure out, how to annotate the lifetime of reference, so it asked programmer to do the annotation.

### Problem 1: Borrowed types in a function

```rs
// Error - missing lifetime specifier
fn longest(x: &str, y: &str) -> &str {
  if x.len() > y.len() {
    x
  } else {
    y
  }
}
```

Compiler is unable to annotate the `x` and `y` implicitly because either `x` or `y` as reference is returned based on the if condition. Compiler does not figure out which reference is going to returned `x` or `y` for sure, it could be any one of them at the runtime.

As you know, to validate and check the lifetime of returned reference as in example above, compiler uses the borrow checker but it cannot annotate the references in function parameter with lifetime annotaton because Rust can’t tell whether the reference being returned refers to `x` or `y`. The borrow checker can’t determine this either, because it doesn’t know how the lifetimes of `x` and `y` relate to the lifetime of the return value.

#### Explicit annotation

```rs
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
  if x.len() > y.len() {
    x
  } else {
    y
  }
}
// We are just telling the compiler that in order to
// prevent dangling reference of reference returned
// which is either `x` or `y` in relation to function
// parameters `x` and `y`, all (parameters `x`, `y` and retruned ref)
// must have same lifetime. If anyone of them has short lifetime
// then it cause a dangling reference.
```

The function signature now tells Rust that for some lifetime `'a`, the function takes two parameters, both of which are string literals that live at least as long as lifetime `'a`. The function signature also tells Rust that the string literal returned from the function will live at least as long as lifetime `'a`. These constraints are what we want Rust to enforce. Remember, when we specify the lifetime parameters in this function signature, we’re not changing the lifetimes of any values passed in or returned. Rather, we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints.

#### Not every parameter is required to annotate

> Ultimately, lifetime syntax is about connecting the lifetimes of various parameters and return values of functions. Once they’re connected, Rust has enough information to allow memory-safe operations and disallow operations that would create dangling pointers or otherwise violate memory safety.

The way in which you need to specify lifetime parameters depends on what your function is doing. For example, if we changed the implementation of the `longest` function to always return the first parameter rather than the longest string slice, we wouldn’t need to specify a lifetime on the `y`parameter. The following code will compile:

```rs
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
  x
}
```

In this example, we’ve specified a lifetime parameter `'a` for the parameter  `x`  and the return type, but not for the parameter `y`, because the lifetime of `y` does not have any relationship with the lifetime of `x` or the return value.

When returning a reference from a function, the lifetime parameter for the return type needs to match the lifetime parameter for one of the parameters. If the reference returned does _not_ refer to one of the parameters, it must refer to a value created within this function, which would be a dangling reference because the value will go out of scope at the end of the function. Consider this attempted implementation of the `longest` function that won’t compile:

```rs
fn longest<'a>(x: &str, y: &str) -> &'a str {
  let result = String::from("really long string");
  // error[E0597]: `result` does not live long enough
  result.as_str()
}
```

Here, even though we’ve specified a lifetime parameter `'a` for the return type, this implementation will fail to compile because the return value lifetime is not related to the lifetime of the parameters at all.

The problem is that `result` goes out of scope and gets cleaned up at the end of the `longest`function. We’re also trying to return a reference to `result` from the function. There is no way we can specify lifetime parameters that would change the dangling reference, and Rust won’t let us create a dangling reference. In this case, the best fix would be to return an owned data type rather than a reference so the calling function is then responsible for cleaning up the value.

### Problem 2: Storing a borrow in a `struct`

So far, we’ve only defined structs to hold owned types. It’s possible for structs to hold references, but in that case we would need to add a lifetime annotation on every reference in the struct’s definition.

```rs
struct Object {
  number: u32
}

struct Multiplier {
  object: &Object,
  mult: u32
}
```

Rust is complaining about not being able to figure your lifetimes out for you. Let’s step through the logic.

- We introduce an `Objct` type,containing a single `number`. Rust is happy with this idea, as it figures that the `nuber` will live as long as the `Object` does.
- We introduce a `Mltiplie` type, containing a `mult` and a **borrow** of an `Object`. Rust is happy about the `mult` - it uses the same logic as above - but it doesn’t like the `object` borrow.
- The thing is, `object` could live for any odd lifetime - possibly one **smaller** than the  `Multiplier`’s! Rust wants us to constrain the `Multiplier` so that it can check your code.

```rs
struct Multiplier<'a> {
  object: &'a Object,
  mult: u32
}
```

Basically, this code says “I have an object here called `Multiplier` that lives as long as the lifetime `'a`. Given that lifetime, I have an `object` inside me that lasts as least as long as `'a`.” This then “links” the lifetime of a `Multiplier` to the lifetime of the `Object` inside it, making sure the `Object` won’t go away before the `Multiplier` does.

## `static` lifetime

It means that this reference can live for the entire duration of the program. All string literals have the `'static` lifetime, which we can annotate as follows:

```rs
let s: &'static str = "I have a static lifetime.";
```

The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is  `'static`.

You might see suggestions to use the  `'static`  lifetime in error messages. But before specifying `'static` as the lifetime for a reference, think about whether the reference you have actually lives the entire lifetime of your program or not. You might consider whether you want it to live that long, even if it could. Most of the time, the problem results from attempting to create a dangling reference or a mismatch of the available lifetimes. In such cases, the solution is fixing those problems, not specifying the `'static` lifetime.
