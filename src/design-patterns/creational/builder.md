# Builder

It lets you **create complex objects step by step**. The pattern allows you to produce different types and representations of an object using the same construction code.

This pattern should be used when object creation is complicated and involves a lot of code. The most common motivation for using Builder is to simplify client code that creates complex objects. The client can still direct the steps taken by the Builder without knowing how the actual work is accomplished.

## Problem

For example, let’s think about how to create a  `House`  object. To build a simple house, you need to construct four walls and a floor, install a door, fit a pair of windows, and build a roof. But what if you want a bigger, brighter house, with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)?

The simplest solution is to extend the base  `House`  class and create a set of subclasses to cover all combinations of the parameters. But eventually you’ll end up with a considerable number of subclasses. Any new parameter, such as the porch style, will require growing this hierarchy even more.

There’s another approach that doesn’t involve breeding subclasses. You can create a giant constructor right in the base  `House`  class with all possible parameters that control the house object. While this approach indeed eliminates the need for subclasses, it creates another problem.

The constructor with lots of parameters has its downside: not all the parameters are needed at all times. In most cases most of the parameters will be unused, making the constructor calls pretty ugly.

```js
new House(4, 2, 4, null, true, false, null, ...)
```

## Solution

The Builder pattern suggests that you extract the object construction code out of class and move it to separate objects called builders.

The pattern organizes object construction into a set of steps (buildWalls, buildDoor, etc.). To create an object, you execute a series of these steps on a builder object. The important part is that you don’t need to call all of the steps. You can call only those steps that are necessary for producing a particular configuration of an object.

Some of the construction steps might require different implementation when you need to build various representations of the product. For example, walls of a cabin may be built of wood, but the castle walls must be built with stone.

In this case, you can create several different builder classes that implement the same set of building steps, but in a different manner. Then you can use these builders in the construction process (i.e., an ordered set of calls to the building steps) to produce different kinds of objects.

## Code

```js
/**
1. Director class which runs a multiple steps to get final object.
  IT JUST RUNS A STEPS AND NOTHING ELSE. Class's constructor accepts
  a builder object and runs series of steps on builder object and return
  final product. In order to work it uniformly each ConcreteBuilder must
  implement same interface because Director does not know anything about
  ConcreteBuilder but only interface by which it communicate.

  IT RUNS STEPS ONE BY ONE.
**/

function Shop() {
  this.construct = function(builder) {
    builder.step1();
    builder.step2();
    return builder.get();
  }
}

/**
2. ConcreteBuilder classes which have same interface (methods).
   They may have any number of different and methods but they must have
  same methods which Director class uses to run steps. The class creates object
  of Product class. ITS MAIN DUTY IS TO USE PRODUCT CLASS MEHTODS AND IMPLEMENT
  STEPS THAT IN TURNS USED BY DIRECTOR CLASS. Finally it creates a Final Product Object.

  IT IMPLEMENTS STEPS.
**/
function CarBuilder() {

  this.car = null;

  this.step1 = function() {
    this.car = new Car();
  };

  this.step2 = function() {
    this.car.addParts();
  };

  this.get = function() {
    return this.car;
  };
}

function TruckBuilder() {
  this.truck = null;

  this.step1 = function() {
    this.truck = new Truck();
  };

  this.step2 = function() {
    this.truck.addParts();
  };

  this.get = function() {
    return this.truck;
  };
}

/**
 * 3. Product class provide mehtods and properties to be used by builder
 * to implement steps.
 *
 * IT PROVIDE NECESSARY METHODS AND PROPERTIES TO IMPLEMENT STEPS
 * REQUIRED BY ConcreteBuilder CLASS.
 */
function Car() {

  this.doors = 0;

  this.addParts = function() {
    this.doors = 4;
  };

  this.say = function() {
    console.log("I am a " + this.doors + "-door car");
  };
}

function Truck() {

  this.doors = 0;

  this.addParts = function() {
    this.doors = 2;
  };

  this.say = function() {
    console.log("I am a " + this.doors + "-door truck");
  };
}

/**
 * 4. Client
 */
function run() {

  var shop = new Shop();
  var carBuilder = new CarBuilder();
  var truckBuilder = new TruckBuilder();
  var car = shop.construct(carBuilder);
  var truck = shop.construct(truckBuilder);

  car.say();
  truck.say();

}

/**
 * Execute
 */

run();
```
