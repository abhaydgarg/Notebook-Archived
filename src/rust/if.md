# if - else if - else

```rs
let team_size = 7;
if team_size < 5 {
  println!("Small");
} else if team_size < 10 {
  println!("Medium");
} else {
  println!("Large");
}

//# Returning
// Return data type should be the same on each
// block when using this.
let team_size = 7;
let team_size_in_text = if team_size < 5 {
    "Small"
} else if team_size < 10 {
    "Medium"
} else {
    "Large"
}; // Don't forget to put a semicolon here!
println!("Current team size : {}", team_size_in_text);
```

## if let

```rs
//# Option

let number = Some(7);
if let Some(i) = number {
  println!("{:?}", i); // 7
} else {
  // Handle `None` here.
}
```

```rs
//# Result

//1. Handle Ok
let age: Result<u8, _> = "34".parse();
if let Ok(i) = age {
  println!("{:?}", i); // 34
} else {
  // Handle Err here.
  // But you do not have a
  // Err value returned by Result.
}

//2. Handle Err
let age: Result<u8, _> = "34a".parse();
if let Err(e) = age {
  println!("{:?}", e); // ParseIntError { kind: InvalidDigit }
} else {
  // Handle Ok here.
  // But you do not have
  // Ok value returned by Result.
}
```

### Combination of Option and Result

```rs
fn main() {
  let favorite_color: Option<&str> = None;
  let is_tuesday = false;
  let age: Result<u8, _> = "34".parse();

  if let Some(color) = favorite_color {
    println!("Using your favorite color, {}, as the background", color);
  } else if is_tuesday {
    println!("Tuesday is green day!");
  } else if let Ok(age) = age {
    if age > 30 {
      println!("Using purple as the background color");
    } else {
      println!("Using orange as the background color");
    }
  } else {
    println!("Using blue as the background color");
  }
}
```
