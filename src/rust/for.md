# for

```rs
// 0 is inclusive and 10 is exclusive
// 0 to 9
for a in 0..10 { // (a = 0; a < 10; a++)
  println!("Current value : {}", a);
}

// 0 is inclusive and 10 is inclusive too
// 0 to 10
for a in 0..=10 { // (a = 0; a <= 10; a++)
  println!("Current value : {}", a);
}

//# Outer break
'outer_for: for c1 in 1..6 { // set label outer_for
  'inner_for: for c2 in 1..6 {
    println!("Current Value : [{}][{}]", c1, c2);
    if c1 == 2 && c2 == 2 { break 'outer_for; } // kill outer_for
  }
}

//# Working with arrays/vectors
let group : [&str; 4] = ["Mark", "Larry", "Bill", "Steve"];

// 👎 group.len() = 4 -> 0..4
// check group.len() on each iteration
for n in 0..group.len() {
  println!("Current Person : {}", group[n]);
}

// 👍 group.iter() turn the array into a simple iterator
for person in group.iter() {
  println!("Current Person : {}", person);
}
```

## Pattern to destructure a tuple as part of the for loop

```rs
let v = vec!['a', 'b', 'c'];

for (index, value) in v.iter().enumerate() {
  println!("{} is at index {}", value, index);
}
```
