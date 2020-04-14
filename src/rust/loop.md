# loop

Rust provides a `loop` keyword to indicate an **infinite loop**.

The `break` statement can be used to exit a loop at anytime, whereas the `continue` statement can be used to skip the rest of the iteration and start a new one.

```rs
let mut a = 0;
loop {
  if a == 0 {
    println!("Skip Value : {}", a);
    a += 1;
    continue;
  } else if a == 2 {
    println!("Break At : {}", a);
    break;
  }
  println!("Current Value : {}", a);
  a += 1;
}
```

## Nesting and lables

It's possible to `break` or `continue` outer loops when dealing with nested loops. In these cases, the loops must be annotated with some 'label, and the label must be passed to the break/continue statement.

```rs
// Outer break
let mut b1 = 1;
'outer_loop: loop { // set label outer_loop
  let mut b2 = 1;
  'inner_loop: loop {
    println!("Current Value : [{}][{}]", b1, b2);
    if b1 == 2 && b2 == 2 {
      break 'outer_loop; // kill outer_loop
    } else if b2 == 5 {
      break;
    }
    b2 += 1;
  }
  b1 += 1;
}
```

## Returning from loop

One of the uses of a `loop` is to retry an operation until it succeeds. If the operation returns a value though, you might need to pass it to the rest of the code: put it after the `break`, and it will be returned by the loop expression.

```rs
let mut counter = 0;

let result = loop {
  counter += 1;

  if counter == 10 {
    break counter * 2; // returning
  }
};

assert_eq!(result, 20);
```
