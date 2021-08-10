# Factory

A Factory Method creates new objects as instructed by the client. One way to create objects in JavaScript is by invoking a constructor function with the new operator. There are situations however, where the client does not, or should not, know which one of several candidate objects to instantiate. The Factory Method allows the client to delegate object creation while still retaining control over which type to instantiate.

## Problem

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the Truck class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

Great news, right? But how about the code? At present, most of your code is coupled to the Truck class. Adding Ships into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

## Applicability

1. Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.

2. Loose Coupling and Segregation of Responsibilities. Now instead of hard binding the complete logic to decide the nature and shape of the object on the basis of some conditions, you are assigning the responsibility to some other object and hence making the relationship loosely coupled and hence main tenable.

3. Great for generating different objects based on the environment.

## Code

```js
/**
1. Factory class
**/

function Factory() {

  // Factory method that handles the creation logic of
  // an object based on type consdition
  this.createEmployee = function(type) {
    var employee;

    if (type === "fulltime") {
      employee = new FullTime();
    } else if (type === "parttime") {
      employee = new PartTime();
    } else if (type === "temporary") {
      employee = new Temporary();
    } else if (type === "contractor") {
      employee = new Contractor();
    }

    return employee;
  }
}

/**
2. Employee classes, see all of them have same interface(properties and methods).
  You must ensure that all classes created by factory class have same interface.
  Javascript does not supoort Abstract classes otherwise interface sameness can
  be ensure by using Abstract class.Classes may have additional properties and
  methods but all classes must have or implement `hourly`, `type` and `say`.
**/

var FullTime = function() {
  this.hourly = "$12";
  this.type = "Full Time";
  this.say = function() {
    console.log(this.type + ": rate " + this.hourly + "/hour");
  };
};

var PartTime = function() {
  this.hourly = "$11";
  this.type = "Part Time";
  this.say = function() {
    console.log(this.type + ": rate " + this.hourly + "/hour");
  };
};

/**
3. Client or function that uses employee classes which actually does
   not know about how product is created and also does not know about type
  of product.Advantage of keeping same interface in all employee classes now
  we can without any worry call `say()` method and `run()` client need not
  to make any decision on that, that whatever object is returned by
  factory class calling `say()`will work.
**/

function run() {

  var factory = new Factory();

  factory.createEmployee("fulltime").say();
  factory.createEmployee("parttime").say();

}

run();
```
