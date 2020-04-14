# while

```rs
let mut b = 0;
while b < 5 {
  if b == 0 {
    println!("Skip value : {}", b);
    b += 1;
    continue;
  } else if b == 2 {
    println!("Break At : {}", b);
    break;
  }
  println!("Current value : {}", b);
  b += 1;
}
```

## Nesting and lables

It's possible to `break` or `continue` outer loops when dealing with nested loops. In these cases, the loops must be annotated with some 'label, and the label must be passed to the break/continue statement.

```rs
// Outer break
let mut c1 = 1;
'outer_while: while c1 < 6 { // set label outer_while
  let mut c2 = 1;
  'inner_while: while c2 < 6 { // set label inner_while
    println!("Current Value : [{}][{}]", c1, c2);
    if c1 == 2 && c2 == 2 { break 'outer_while; } // kill outer_while
    c2 += 1;
  }
  c1 += 1;
}
```

## while let

Similar in construction to `if let`, the `while let` conditional loop allows a `while` loop to run for as long as a pattern continues to match.

```rs
//# Option
let mut stack = Vec::new();

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop() {
  println!("{}", top);
}
```

The `pop` method takes the last element out of the vector and returns `Some(value)`. If the vector is empty, `pop` returns `None`. The `while` loop continues running the code in its block as long as `pop` returns `Some`.
