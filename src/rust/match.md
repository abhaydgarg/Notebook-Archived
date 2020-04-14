# match

> Like a `switch`

A particular pattern `_` will match anything, but it never binds to a variable, so it’s often used in the last match arm.

```rs
let tshirt_width = 20;
let tshirt_size = match tshirt_width {
  16 => "S", // check 16
  17 | 18 => "M", // check 17 and 18
  19 ... 21 => "L", // check from 19 to 21 (19,20,21)
  22 => "XL",
  _ => "Not Available", // Default when nothing match
};
println!("{}", tshirt_size); // L

//# Tuple destructuring
let pair = (0, -2);

match pair {
  (0, y) => println!("First is `0` and `y` is `{:?}`", y),
  (x, 0) => println!("`x` is `{:?}` and last is `0`", x),
  _      => println!("It doesn't matter what they are"),
}
// Output: First is `0` and `y` is `-2`
```

## Guard

> Add filter.

See the `if` condition.

```rs
let pair = (2, -2);
match pair {
  (x, y) if x == y => println!("These are twins"),
  (x, y) if x + y == 0 => println!("Antimatter, kaboom!"),
  (x, _) if x % 2 == 1 => println!("The first one is odd"),
  _ => println!("No correlation..."),
}
// Output: Antimatter, kaboom!

let marks_paper_a: u8 = 25;
let marks_paper_b: u8 = 30;
let output = match (marks_paper_a, marks_paper_b) {
  (50, 50) => "Full marks for both papers",
  (50, _) => "Full marks for paper A",
  (_, 50) => "Full marks for paper B",
  (x, y) if x > 25 && y > 25 => "Good",
  (_, _) => "Work hard"
}; // Don't forget to put a semicolon here!
println!("{}", output); // Work hard
```

## Option

```rs
fn divide(numerator: f64, denominator: f64) -> Option<f64> {
  if denominator == 0.0 {
    None
  } else {
    Some(numerator / denominator)
  }
}

// The return value of the function is an option.
let result = divide(2.0, 3.0);

// Pattern match to retrieve the value.
match result {
  // The division was valid.
  Some(x) => println!("Result: {}", x),
  // The division was invalid.
  None    => println!("Cannot divide by 0"),
}
```

## Result

```rs
#[derive(Debug)]
enum Version { Version1, Version2 }

fn parse_version(header: &[u8]) -> Result<Version, &'static str> {
  match header.get(0) {
    None => Err("invalid header length"),
    Some(&1) => Ok(Version::Version1),
    Some(&2) => Ok(Version::Version2),
    Some(_) => Err("invalid version"),
  }
}

let version = parse_version(&[1, 2, 3, 4]);
match version {
  Ok(v) => println!("working with version: {:?}", v),
  Err(e) => println!("error parsing header: {:?}", e),
}
```
