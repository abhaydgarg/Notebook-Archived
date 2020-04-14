# Impls & Traits

> **impls** are **used to define methods** for Rust structs and enums.

> **Traits** are kind of **similar to interfaces** in OOP languages. They are used to define the functionality a type must provide. Multiple traits can be implemented for a single type. But traits **can also include default implementations of methods**. Default methods can be overridden when implementing types.

## self, &self, &mut self

> `self` is like a `this`.

It can be **either self, &self, or &mut self**; `self` if it’s a value on the stack (taking ownership), `&self` if it’s a reference, and `&mut self` if it’s a mutable reference.

## Impls without traits

```rs
struct Player {
  first_name: String,
  last_name: String,
}

impl Player {
  fn full_name(&self) -> String {
    format!("{} {}", self.first_name, self.last_name)
  }
}

fn main() {
  let player_1 = Player {
    first_name: "Rafael".to_string(),
    last_name: "Nadal".to_string(),
  };

  println!("Player 01: {}", player_1.full_name());
}
```

## Impls & traits, without default methods

```rs
struct Player {
  first_name: String,
  last_name: String,
}

// Like a interface
trait FullName {
  fn full_name(&self) -> String;
}

// Implementing interface FullName
impl FullName for Player {
  fn full_name(&self) -> String {
    format!("{} {}", self.first_name, self.last_name)
  }
}

fn main() {
  let player_2 = Player {
    first_name: "Roger".to_string(),
    last_name: "Federer".to_string(),
  };

  println!("Player 02: {}", player_2.full_name());
}
```

## Impls, traits & default methods

```rs
trait Foo {
  fn bar(&self);
  // Can include default implementations of method
  // unlike Interface in OOPS.
  fn baz(&self) {
    println!("We called baz.");
  }
}
```

## Static methods (Associated Functions)

**Call a function directly** through the class without creating an object. In Rust, we call them **Associated Functions**. we use `::` instead of `.` when calling them from the struct.

> Associated functions are often used for constructors that will return a new instance of the struct. `Person::new(“Elon Musk Jr”);`

```rs
struct Player {
  first_name: String,
  last_name: String,
}

impl Player {
  // Don’t take self as a parameter
  fn new(first_name: String, last_name: String) -> Player {
    Player {
      first_name : first_name,
      last_name : last_name,
    }
  }

  fn full_name(&self) -> String {
    format!("{} {}", self.first_name, self.last_name)
  }
}

fn main() {
  // We have used :: notation for `new()` and . notation for `full_name()`
  let player_name = Player::new("Serena".to_string(), "Williams".to_string()).full_name();
  println!("Player: {}", player_name);
}
```

## Traits inheritance

```rs
trait Person {
  fn full_name(&self) -> String;
}

// Employee inherits from person trait
trait Employee : Person {
  fn job_title(&self) -> String;
}

// ExpatEmployee inherits from Employee and Expat traits
trait ExpatEmployee : Employee + Expat {
  fn additional_tax(&self) -> f64;
}
```
