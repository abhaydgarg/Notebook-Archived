# Functions

## Default parameter

```js
// ###### ECMAScript 5 ######
function makeRequest(url, timeout, callback) {

    timeout = timeout || 2000;
    callback = callback || function() {};

    // the rest of the function

}

// ###### ECMAScript 6 ######

function makeRequest(url, timeout = 2000, callback = function() {}) {

    // the rest of the function

}

// if you pass undefined or null for default parameter then also you get default
// value not undefined or null.

// uses default timeout = 2000, even undefined is passed
makeRequest("/foo", undefined, function(body) {
    doSomething(body);
});

// uses default timeout
makeRequest("/foo");

// doesn't use default timeout  = 2000, even null is passed
makeRequest("/foo", null, function(body) {
    doSomething(body);
});
```

### How default parameter affect the argument object

```js
// ###### ECMAScript 5 ######
// 1. nonstrict mode, the arguments object reflects changes
// in the named parameters of a function

function mixArgs(first, second) {
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d";
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a", "b");

//This outputs:

true
true
true
true

// 2. strict mode, the arguments object does not reflect
// changes in the named parameters of a function
function mixArgs(first, second) {
    "use strict";

    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d"
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a", "b");

// The call to mixArgs() outputs: This time, changing first
// and second doesn’t affect arguments,
// so the output behaves as you’d normally expect it to.

true
true
false
false


// ###### ECMAScript 6 ######
// The arguments object in a function using ECMAScript 6 default parameter values,
// however, will always behave in the same manner as ECMAScript 5 strict mode.

// Note: Default value does not attach to the arguments object.

function mixArgs(first, second = "b") {
    console.log(arguments.length);
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d"
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a");

// outputs:

1
true
false
false
false

// arguments.length is 1 because only one argument was passed to mixArgs().
// That also means arguments[1] is undefined and default value b
// is not attached to arguments object.
```

### Default parameter expressions

Perhaps the most interesting feature of default parameter values is that the default value need not be a primitive value. You can, for example, execute a function to retrieve the default parameter value, like this:

```js
function getValue() {
    return 5;
}

function add(first, second = getValue()) {
    return first + second;
}

console.log(add(1, 1));     // 2
console.log(add(1));        // 6

// Be careful when using function calls as default parameter values.
// If you forget the parentheses, such as second = getValue in the last example,
// you are passing a reference to the function rather than the result of the function call.
```

## Unnamed parameters

JavaScript functions don't limit the number of parameters that can be passed to the number of named parameters defined. You can always pass fewer or more parameters than formally specified.

```js
// ### ECMAScript 5 ###
function pick(object) {
    let result = Object.create(null);

    // start at the second parameter
    for (let i = 1, len = arguments.length; i < len; i++) {
        result[arguments[i]] = object[arguments[i]];
    }

    return result;
}

let book = {
    title: "Understanding ECMAScript 6",
    author: "Nicholas C. Zakas",
    year: 2015
};

let bookData = pick(book, "author", "year");

console.log(bookData.author);   // "Nicholas C. Zakas"
console.log(bookData.year);     // 2015


// ### ECMAScript 6 ###

// Rest Parameters: A rest parameter is indicated by three dots (...)
// preceding a named parameter.
// That named parameter becomes an Array containing the rest of the parameters passed to
// the function, which is where the name “rest” parameters originates.
// For example, pick() can be rewritten using rest parameters, like this:

function pick(object, ...keys) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```

### Rest parameter restrictions

1. There can be only one rest parameter, and the rest parameter must be last.

2. Rest parameters cannot be used in an object literal setter.

```js
let object = {

    // Syntax error: Can't use rest param in setter
    set name(...value) {
        // do something
    }
};

//This restriction exists because object literal setters are restricted to
// a single argument. Rest parameters are, by definition, an infinite number of
// arguments, so they’re not allowed in this context.
```

#### How rest parameters affect the argument object

```js
function checkArgs(...args) {
    console.log(args.length);
    console.log(arguments.length);
    console.log(args[0], arguments[0]);
    console.log(args[1], arguments[1]);
}

checkArgs("a", "b");

// The call to checkArgs() outputs:

2
2
a a
b b

// The arguments object always correctly reflects the parameters
// that were passed into a function
// regardless of rest parameter usage.
```

## Dual purpose of functions

JavaScript has two different internal-only methods for functions: `[[Call]]` and `[[Construct]]`. When a function is called without new, the [[Call]] method is executed, which executes the body of the function as it appears in the code. When a function is called with new, that's when the [[Construct]] method is called. The [[Construct]] method is responsible for creating a new object, called the new target, and then executing the function body with this set to the new target. Functions that have a [[Construct]] method are called constructors.

```js
// ### ECMAScript 5 ###

function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");  // throws error

// Here, the this value is checked to see if it’s an instance of the constructor,
// and if so, execution continues as normal. If this isn’t an instance of Person,
// then an error is thrown. This works because the [[Construct]] method creates
// a new instance of Person and assigns it to this. Unfortunately, this approach
// is not completely reliable because this can be an instance of Person without using
// new, as in this example:

function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // works!

// The call to Person.call() passes the person variable as the first argument,
// which means this is set to person inside of the Person function. To the function,
// there’s no way to distinguish this from being called with new.
```

To solve this problem, ECMAScript 6 introduces the `new.target` metaproperty. A metaproperty is a property of a non-object that provides additional information related to its target (such as new). When a function's [[Construct]] method is called, `new.target` is filled with the target of the new operator. That target is typically the constructor of the newly created object instance that will become this inside the function body. If [[Call]] is executed, then `new.target` is undefined.

```js
// ### ECMAScript 6 ###

function Person(name) {
    if (typeof new.target !== "undefined") {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // error!
```

> Using `new.target` outside of a function is a syntax error.

## Arrow functions

Arrow functions behave differently than traditional JavaScript functions in a number of important ways:

* **No this, super, arguments, and new.target bindings**.
* **Cannot be called with new** - Arrow functions do not have a `[[Construct]]` method and therefore cannot be used as constructors. Arrow functions throw an error when used with `new`.
* **No prototype** - since you can't use `new` on an arrow function, there's no need for a prototype. The `prototype` property of an arrow function doesn't exist.
* **Can't change this** - The value of `this` inside of the function can't be changed. It remains the same throughout the entire lifecycle of the function.
* **No arguments object** - Since arrow functions have no `arguments` binding, you must rely on named and rest parameters to access function arguments.
* **No duplicate named parameters** - arrow functions cannot have duplicate named parameters in strict or nonstrict mode, as opposed to non arrow functions that cannot have duplicate named parameters only in strict mode.

> There are a few reasons for these differences. First and foremost, `this` binding is a common source of error in JavaScript. It's very easy to lose track of the `this` value inside a function, which can result in unintended program behavior, and arrow functions eliminate this confusion. Second, by limiting arrow functions to simply executing code with a single `this` value, JavaScript engines can more easily optimize these operations

```js
// ### When you want to provide a one expression which is return statement, then you no need
// to wrap the function body in braces and no need to define a return value.

var reflect = value => value;

/**
var reflect = function(value) {
    return value;
};
**/

var sum = (num1, num2) => num1 + num2;

/**
var sum = function(num1, num2) {
    return num1 + num2;
};
**/


var getName = () => "Nicholas";

/**
var getName = function() {
    return "Nicholas";
};
**/


// ### When you want to provide a more traditional function body,
// perhaps consisting of more than one
// expression, then you need to wrap the function body in braces
// and explicitly define a return value.

var sum = (num1, num2) => {
    return num1 + num2;
};

/**
var sum = function(num1, num2) {
    return num1 + num2;
};
**/

// ### If you want to create a function that does nothing,
// then you need to include curly braces, like this:

var doNothing = () => {};

/**
var doNothing = function() {};
**/

// ### Return an object literal

var getTempItem = id => ({ id: id, name: "Temp" });

/**
var getTempItem = function(id) {

    return {
        id: id,
        name: "Temp"
    };
};
**/

// ### Creating Immediately-Invoked Function Expressions

let person = ((name) => {

    return {
        getName: function() {
            return name;
        }
    };

})("Nicholas");

console.log(person.getName());      // "Nicholas"

// ### In Parameter

var result = values.sort((a, b) => a - b);

/**
var result = values.sort(function(a, b) {
    return a - b;
});
**/
```

### No this binding

One of the most common areas of error in JavaScript is the binding of this inside of functions. Since the value of this can change inside a single function depending on the context in which the function is called.

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        }, false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};

// In this code, the object PageHandler is designed to handle interactions on the page.
// The init() method is called to set up the interactions, and that method in turn assigns
// an event handler to call this.doSomething(). However, this code doesn’t work exactly
// as intended.
// The call to this.doSomething() is broken because this is a reference to the object that
// was the target of the event (in this case document), instead of being bound to PageHandler.
// If you tried to run this code, you’d get an error when the event handler fires because
// this.doSomething() doesn’t exist on the target document object.

// ### You could fix this by binding the value of this to PageHandler explicitly
// using the bind() method on the function instead, like this:

var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);     // no error
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};

// Now the code works as expected, but it may look a little bit strange.
// By calling bind(this), you’re actually creating a new function whose this
// is bound to the current this, which is PageHandler.
// To avoid creating an extra function, a better way to fix this code is
// to use an arrow function.

// ### Arrow functions have no this binding, which means the value of this
// inside an arrow function can only be determined by looking up the scope chain.
// If the arrow function is contained within a nonarrow function, this will
// be the same as the containing function; otherwise, this is undefined.
// Here’s one way you could write this code using an arrow function:

var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
            event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};

// The event handler in this example is an arrow function that calls this.doSomething().
// The value of this is the same as it is within init(), so this version of the code
// works similarly to the one using bind(this). Even though the doSomething()
// method doesn’t return a value, it’s still the only statement executed in the
// function body, and so there is no need to include braces.

// Also, since the this value is determined by the containing function in which
// the arrow function is defined, you cannot change the value of this using
// call(), apply(), or bind().
```

### When not to use arrow function

**Inside an object literal.**

```js
var Actor = {
  name: 'RajiniKanth',
  movies: ['Kabali', 'Sivaji', 'Baba'],
  getName: () => {
     alert(this.name);
  }
};

Actor.getName();
```

`Actor.getName` is defined with an arrow function, but on invocation it alerts undefined because `this.name` is `undefined` as the context remains to `window`.

It happens because the arrow function binds the context lexically with the `window object`... i.e outer scope. Executing `this.name` is equivalent to `window.name`, which is undefined.

#### Object prototype

The same rule applies when defining methods on a `prototype object`.

```js
function Actor(name) {
  this.name = name;
}
Actor.prototype.getName = () => {
  console.log(this === window); // => true
  return this.name;
};
var act = new Actor('RajiniKanth');
act.getName(); // => undefined
```

##### Callback with dynamic context

Arrow function binds the `context` statically on declaration and is not possible to make it dynamic. Attaching event listeners to DOM elements is a common task in client side programming. An event triggers the handler function with this as the target element.

```js
var button = document.getElementById('myButton');
button.addEventListener('click', () => {
  console.log(this === window); // => true
  this.innerHTML = 'Clicked button';
});
```

`this` is window in an arrow function that is defined in the global context. When a click event happens, browser tries to invoke the handler function with button context, but arrow function does not change its pre-defined context. `this.innerHTML` is equivalent to `window.innerHTML` and has no sense.
