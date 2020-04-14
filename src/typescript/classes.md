# Classes

```typescript
class MyClass {

  x: string; // no modifier then `public` by default
  public y: string; // can use public explicitly
  private z: number // private class property
  protected a: boolean // protected

  constructor(message: string) {
    this.x = message;
  }

  greet() {
    return "Hello, " + this.greeting;
  }

  private set() {

  }

  // OR

  public constructor(message: string) {
    this.x = message;
  }

  public greet() {
    return "Hello, " + this.greeting;
  }

  private set() {

  }

}

let myClass = new MyClass("world");
```

## Access modifiers

`public` **by default**

`private` **cannot access outside class**

```typescript
class Animal {
  private name: string;
  constructor(theName: string) {
   this.name = theName;
  }
}

new Animal("Cat").name; // Error: 'name' is private;
```

`protected` **members behave like private with the exception that subclass instances can access them but not instantiated object of subclass directly.** For example:

```typescript
class Person {
  protected name: string;
  constructor(name: string) { this.name = name; }
}

class Employee extends Person {
  private department: string;

  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }

  // `name` from `Person` parent class is protected but yet access by
  // `Employee` subclass instance method
  public getElevatorPitch() {
    return `Hello, my name is ${this.name} and I work in ${this.department}.`;
  }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name);
// error, the protected member of parent class cannot be
// accessed by instantiated object of subclass directly
```

**Make constructor protected**
This means that the class cannot be instantiated outside of its containing class, but can be extended. For example:

```typescript
class Person {
  protected name: string;
  protected constructor(theName: string) { this.name = theName; }
}

// Employee can extend Person
class Employee extends Person {
  private department: string;

  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }

  public getElevatorPitch() {
    return `Hello, my name is ${this.name} and I work in ${this.department}.`;
  }
}

let howard = new Employee("Howard", "Sales");
let john = new Person("John"); // Error: The 'Person' constructor is protected
```

## Inheritance

Each subclass class that contains a constructor function must call `super()` which will execute the constructor of the parent class. What’s more, before we ever access a property on `this` in a constructor body, we have to call `super()`.

```typescript
class Animal {
  move(distanceInMeters: number = 0) {
    console.log(`Animal moved ${distanceInMeters}m.`);
  }
}

class Dog extends Animal {

  constructor(sound: string) {
    super(); // must call if subclass has constructor
     = sound;
  }

  bark() {
    console.log(this.sound);
  }
}

const dog = new Dog('Woof! Woof!');
dog.bark(); // Woof! Woof!
```

## Readonly properties

Readonly properties must be initialized at their declaration or in the constructor.

```typescript
class Octopus {
  readonly name: string;
  readonly numberOfLegs: number = 8;
  constructor(theName: string) {
    this.name = theName;
  }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // error! name is readonly.
```

## Using access modifier in constructor's parameters

```typescript
//# Common way of defining class properties and
// initializing them in constructor is:

class Octopus {
  private name: string; // define property
  constructor(theName: string) {
    // getting value from `theName` and set to `this.name`
    this.name = theName;
  }
}
let dad = new Octopus("Man with the 8 strong legs");

//# you can use access modifiers in constructor's parameters to eliminate
// defining class property separately

class Octopus {
  constructor(private name: string) {
    console.log(this.name);
  }
}
let dad = new Octopus("Man with the 8 strong legs");

// likewise, the same is done for public, protected, and readonly.
```

## Static

```typescript
class Grid {
  static origin = { x: 0, y: 0 };
  calculateDistanceFromOrigin(point: { x: number;y: number; }) {
    // `origin` property is accessed by class name than `this`
    let xDist = (point.x - Grid.origin.x);
    let yDist = (point.y - Grid.origin.y);
    return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
  }
  constructor(public scale: number) {}
}

let grid1 = new Grid(1.0);
console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```

## Abstract class

Abstract classes are base classes from which other classes may be derived. The `abstract` keyword is used to define abstract classes as well as abstract methods within an abstract class. Abstract method can have access modifiers too.

```typescript
abstract class Department {

  constructor(public name: string) {}

 // can have regular method
  printName(): void {
    console.log("Department name: " + this.name);
  }

  abstract printMeeting(): void; // must be implemented in derived classes
}

class AccountingDepartment extends Department {

  constructor() {
    super("Accounting and Auditing");
  }

  // abstract method from parent class - implementation
  printMeeting(): void {
    console.log("The Accounting Department meets each Monday at 10am.");
  }

  generateReports(): void {
    console.log("Generating accounting reports...");
  }
}

department = new Department(); // error: cannot create an instance of an abstract class
accountingDepartment = new AccountingDepartment(); // ok
```
