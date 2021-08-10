# Expanded object functionality

## Property initializer shorthand

```js
// ECMAScript 5
function createPerson(name, age) {
    return {
        name: name,
        age: age
    };
}

// ECMAScript 6 - When an object property name is the same
// as the local variable name, you can simply include the
// name without a colon and value.

function createPerson(name, age) {
    return {
        name,
        age
    };
}
```

## Concise method

```js
// ECMAScript 5
var person = {
    name: "Nicholas",
    sayName: function() {
        console.log(this.name);
    }
};

// ECMAScript 6 - the syntax is made more concise by eliminating
// the colon and the function keyword.

var person = {
    name: "Nicholas",
    sayName() {
        console.log(this.name);
    }
};
```

## Computed property names - Dynamic keys

### Dynamic Keys

Where name of the key is not known ahead of time and will be computed and stores in some variable. The value stored in that variable will become the name of object's key.

The square brackets allow you to specify property names using variables and string literals that may contain characters that would cause a syntax error if used in an identifier. This pattern works for property names that are known ahead of time and can be represented with a string literal. If, however, the property name `"first name"` were contained in a variable or had to be calculated, then there would be no way to define that property using an object literal in ECMAScript 5.

In ECMAScript 6, computed property names are part of the object literal syntax, and they use the same square bracket notation that has been used to reference computed property names in object instances. For example:

```js
var lastName = "last name";
var person = {
    "first name": "Nicholas",
    [lastName]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"

/*** OR ***/

var suffix = " name";
var person = {
    ["first" + suffix]: "Nicholas",
    ["last" + suffix]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person["last name"]);       // "Zakas"
```

## Duplicate object literal properties

```js
// ECMAScript 5
"use strict";

var person = {
    name: "Nicholas",
    name: "Greg"        // syntax error in ES5 strict mode
};

// ECMAScript 6
"use strict";

var person = {
    name: "Nicholas",
    name: "Greg"        // no error in ES6 strict mode
};

console.log(person.name);       // "Greg"
```

## Changing an object's prototype

`Object.setPrototypeOf()` method, which allows you to change the prototype of any given object.

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

// prototype is person
let friend = Object.create(person);
console.log(friend.getGreeting());                      // "Hello"
console.log(Object.getPrototypeOf(friend) === person);  // true

// set prototype to dog
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());                      // "Woof"
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

The actual value of an object's prototype is stored in an internal-only property called `[[Prototype]]`. The `Object.getPrototypeOf()`method returns the value stored in `[[Prototype]]` and `Object.setPrototypeOf()` changes the value stored in `[[Prototype]]`.

## Prototype access with `Super`

Another improvement is the introduction of `super` references, which make accessing functionality on an object's prototype easier. For example, to override a method on an object instance such that it also calls the prototype method of the same name, you'd do the following:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

let friend = {
    // override method - method with the same name calling prototype method of same name
    getGreeting() {
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};

// set prototype to person
Object.setPrototypeOf(friend, person);
console.log(person.getGreeting());                      // "Hello"
console.log(friend.getGreeting());                      // "Hello, hi!"
console.log(Object.getPrototypeOf(friend) === person);  // true

// In this example, getGreeting() on friend calls the prototype method of the same name.
// The Object.getPrototypeOf() method ensures the correct prototype is called,
// and then an additional
// string is appended to the output. The additional .call(this) ensures that
// the this value inside the prototype method is set correctly.
```

The use `Object.getPrototypeOf()` and `.call(this)` to call a method on the prototype is simplified by keyword `super`. Above code can be rewrite as:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

let friend = {
    // override method - method with the same name calling prototype method of same name
    getGreeting() {
      // return Object.getPrototypeOf(this).getGreeting.call(this);
        return super.getGreeting() + ", hi!";
    }
};

// set prototype to person
Object.setPrototypeOf(friend, person);
console.log(person.getGreeting());                      // "Hello"
console.log(friend.getGreeting());                      // "Hello, hi!"
console.log(Object.getPrototypeOf(friend) === person);  // true
```

### `super` works with concise method only

Attempting to use `super` outside of concise methods results in a syntax error, as in this example:

```js
let friend = {
    getGreeting: function() { // not a concise method
        // syntax error
        return super.getGreeting() + ", hi!";
        // super is undefined because scope changed inside named function and super does
        // not exist anywhere in context.
    }
};

// This example uses a named property with a function, and the call to
// super.getGreeting() results in a syntax error because super is invalid
// or undefined in this context.
```

### `super` and multilevel inheritance

The `super` reference is really powerful when you have multiple levels of inheritance, because in that case, `Object.getPrototypeOf()` no longer works in all circumstances

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);


// prototype is friend
let relative = Object.create(friend);

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // error!
```

The call to `Object.getPrototypeOf()` results in an error when `relative.getGreeting()` is called. That's because `this` is `relative`, and the prototype of `relative` is the `friend` object. When `friend.getGreeting().call()` is called with `relative` as `this`, the process starts over again and continues to call recursively until a stack overflow error occurs.

That problem is difficult to solve in ECMAScript 5, but with ECMAScript 6 and `super`, it's easy:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return super.getGreeting() + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);


// prototype is friend
let relative = Object.create(friend);

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // "Hello, hi!"
```

Because `super` references are not dynamic, they always refer to the correct object. In this case, `super.getGreeting()` always refers to `person.getGreeting()`, regardless of how many other objects inherit the method.

### [[HomeObject]]

ECMAScript 6 formally defines a method as a function that has an internal `[[HomeObject]]` property containing the object to which the method belongs.

```js
let person = {

    // method
    getGreeting() {
        return "Hello";
    }
};

// not a method
function shareGreeting() {
    return "Hi!";
}
```

This example defines `person` with a single method called `getGreeting()`. The `[[HomeObject]]` for `getGreeting()` is `person` by virtue of assigning the function directly to an object. The `shareGreeting()` function, on the other hand, has no `[[HomeObject]]`specified because it wasn't assigned to an object when it was created. In most cases, this difference isn't important, but it becomes very important when using `super` references.

Any reference to `super` uses the `[[HomeObject]]` to determine what to do. The first step is to call `Object.getPrototypeOf()` on the `[[HomeObject]]` to retrieve a reference to the prototype. Then, the prototype is searched for a function with the same name. Last, the `this` binding is set and the method is called. Here's an example:

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    getGreeting() {
        return super.getGreeting() + ", hi!";
    }
};
Object.setPrototypeOf(friend, person);

console.log(friend.getGreeting());  // "Hello, hi!"

// The [[HomeObject]] of friend.getGreeting() is friend, and
// the prototype of friend is person, so super.getGreeting()
// is equivalent to person.getGreeting.call(this)`.
```
