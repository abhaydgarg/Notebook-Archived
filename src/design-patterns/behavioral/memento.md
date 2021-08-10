# Momento

It lets you **save and restore the state of an object.**

The Memento pattern provides temporary storage as well as restoration of an object. The mechanism in which you store the object’s state depends on the required duration of persistence, which may vary.

You could view a database as an implementation of the Memento design pattern in which objects are persisted and restored. However, the most common reason for using this pattern is to capture a snapshot of an object’s state so that any subsequent changes can be undone easily if necessary.

Essentially, a Memento is a small repository that stores an object’s state. Scenarios in which you may want to restore an object into a state that existed previously include: saving and restoring the state of a player in a computer game or the implementation of an undo operation in a database.

In JavaScript Mementos are easily implemented by serializing and de-serializing objects with JSON.

## Applicability

Use the Memento pattern when you want to produce snapshots of the object’s state to be able to restore a previous state of the object.

## Code

```js
/**
 * 1. Originator
 */
var Person = function(name, street, city, state) {
  this.name = name;
  this.street = street;
  this.city = city;
  this.state = state;
};

Person.prototype = {

  hydrate: function() {
    //JSON representation of Person is a memento. Therefore no need
    // to have separate class to serialize object, JSON.stringify()
    // act as memento.
    var memento = JSON.stringify(this);
    return memento;
  },

  dehydrate: function(memento) {
    var m = JSON.parse(memento);
    this.name = m.name;
    this.street = m.street;
    this.city = m.city;
    this.state = m.state;
  }
};

/**
 * 2. CareTaker
 */
var CareTaker = function() {
  this.mementos = {};

  this.add = function(key, memento) {
      this.mementos[key] = memento;
    },

    this.get = function(key) {
      return this.mementos[key];
    }
};

/**
 * 3. Client
 */

function run() {
  var mike = new Person("Mike Foley", "1112 Main", "Dallas", "TX");
  var john = new Person("John Wang", "48th Street", "San Jose", "CA");
  var caretaker = new CareTaker();

  // save state

  caretaker.add(1, mike.hydrate());
  caretaker.add(2, john.hydrate());

  // mess up their names

  mike.name = "King Kong";
  john.name = "Superman";

  // restore original state

  mike.dehydrate(caretaker.get(1));
  john.dehydrate(caretaker.get(2));

  console.log(mike.name);
  console.log(john.name);
}

/**
 * 4. execute
 */
run();

```
