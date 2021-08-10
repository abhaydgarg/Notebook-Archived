# Visitor

It lets you separate algorithms from the objects on which they operate. This means that **we can add new operations to the object structure without modifying it.**

The new logic is implemented in a separate object defined as `visitor`. That is useful when building extensibility in a library or a framework. If objects provide a `visit` method accepting the `visitor` object making alterations to the current one, then there is a flawless way for clients to implement future extensions.

Visitor pattern gives us the ability to add much more functionality to an object, without having to modify the object’s class codes.

## Code

```js
/**
 * 1. Class whose functionality is going to add up using Visitor.
 */
var Vessel = function(name, speed, size) {
  this.name = name;
  this.speed = speed;
  this.size = size;

  // must have method that accepts Visitor object
  this.accept = function(visitor) {
    visitor.visit(this);
  };

  this.getName = function() {
    return this.name;
  };

  this.getSpeed = function() {
    return this.speed;
  };

  this.setSpeed = function(speed) {
    this.speed = speed;
  };

  this.getSize = function() {
    return this.size;
  };

  this.setSize = function(size) {
    this.size = size;
  };
};

/**
 * 2. Visitor classes to add feature
 */
var VesselSpeedUp = function() {
  this.visit = function(vessel) {
    vessel.setSpeed(vessel.getSpeed() * 2.5); //2.5 times faster
  };
};

var VesselEnlarge = function() {
  this.visit = function(vessel) {
    vessel.setSize(vessel.getSize() * 2); //twice bigger
  };
};

/**
 * 3. Client
 */
var target = new Vessel("tanker", 25, 350);
var speedUp = new VesselSpeedUp();
var enlarge = new VesselEnlarge();

target.accept(speedUp);
target.accept(enlarge);

console.log(target.getSpeed());
console.log(target.getSize());

```
