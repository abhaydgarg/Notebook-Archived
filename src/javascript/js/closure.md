# Closures

Whenever you see a function within another function, the inner function has access to the scope in the outer function, this is called Closure.

```js
var name = 'Todd';
var scope1 = function () {
  // name is available here
  var scope2 = function () {
    // name is available here too
    var scope3 = function () {
      // name is also available here!
    };
  };
};
```

## A few usage

### [1] Using a closure instead of objects

```js
// Define the constructor
function Person(name, age) {

  // Store the message in internal state
  this.message = name + ", who is " + age + " years old, says hi!";

};

// Define a sync method
Person.prototype.greet = function greet() {
  console.log(this.message);
};

// Define a method with async internals
Person.prototype.slowGreet = function slowGreet() {
  var self = this; // Use a closure to preserve `this`
  setTimeout(function () {
    console.log(self.message);
  }, 1000);
};

// Export this file as a module
module.exports = Person;

// ### Use like this ###

var Person = require('./personclass');
var bob = new Person("Bob", 47);
bob.greet();
```

#### Closures way

```js
// Define the factory
function newPerson(name, age) {

  // Store the message in a closure
  var message = name + ", who is " + age + " years old, says hi!";

  return {

    // Define a sync function
    greet: function greet() {
      console.log(message);
    },

    // Define a function with async internals
    slowGreet: function slowGreet() {
      setTimeout(function () {
        console.log(message);
      }, 1000);
    }

  };
}

// Export this file as a module
module.exports = newPerson;

### Use like this ###
var newPerson = require('./personfactory');
var tim = newPerson("Tim", 28);
tim.greet();
```

The difference is, when using object we can use prototype to make comman features to be shared among objects which saves memory where as in clousres way the whole function context is going to copy and assign to variable.

### [2] Namespacing private functions

Many object-oriented languages provide the ability to declare methods as either public or private. JavaScript doesn’t have this functionality built in, but it does allow to emulate this functionality through the use of closures, which is known as the module pattern.

```js
var dwightSalary = (function() {
    var salary = 60000;
    function changeBy(amount) {
        salary += amount;
    }
    return {
        raise: function() {
            changeBy(5000);
        },
        lower: function() {
            changeBy(-5000);
        },
        currentAmount: function() {
            return salary;
        }
    };
})();

alert(dwightSalary.currentAmount()); // $60,000
dwightSalary.raise();
alert(dwightSalary.currentAmount()); // $65,000
dwightSalary.lower();
dwightSalary.lower();
alert(dwightSalary.currentAmount()); // $55,000

dwightSalary.changeBy(10000) // TypeError: undefined is not a function
```

Using closures to namespace private functions keeps more general namespaces clean, preventing naming collisions. Neither the _salary_ variable nor the _changeBy_ function are available outside of _dwightSalary_. However, _raise, lower_ and _currentAmount_ all have access to them and can be called on _dwightSalary_.

## Scope chain

How variable is looked up. During variable look up, JavaScript starts at the currents scope and searches outwards until it finds the variable/object/function it was looking for.

```js
function foo(a) {
  var b = a * 2;
  function bar(c) {
    console.log( a, b, c );
  }
  bar( b * 3 );
}
foo( 2 ); // 2, 4, 12
```

The engine executes the `console.log(..)` statement and goes looking for the three referenced variables a, b and c . It first starts with the innermost scope, the scope of the `bar(..)` function. It won't find a there, so it goes up one level, out to the next nearest scope, the scope of `foo(..)`. It finds a there, and so it uses that `a` . Same thing for `b`. But `c` , it does find inside of `bar(..)`.

**Note:**

1. Scope look-up stops once it finds the first match.
2. Scope chain tells how closure works.

## Closures’ rules and side effects

### [1] The important thing to remember is that Closure scope does not work backwards

```js
// name = undefined
var scope1 = function () {
  // name = undefined
  var scope2 = function () {
    // name = undefined
    var scope3 = function () {
      var name = 'Todd'; // locally scoped
    };
  };
};
```

### [2] Closures uses only static (lexical) scope for identifier resolution.

```js
var x = 10;

function foo() {
  console.log(x);
}

(function (funArg) {

  var x = 20;

  // variable "x" for funArg is saved statically
  // from the (lexical) context, in which it was created
  // therefore:

  funArg(); // 10, but not 20

})(foo);


// Technically, the variables of a parent context are
// saved in the internal [[Scope]] property of the
// function. All functions in ECMAScript are closures,
// since all of them at creation save scope chain of a
// parent context. The important moment here is that,
// regardless — whether a function will be activated later
// or not — the parent scope is already saved to it at
// creation moment.
```

### [3] Closures have access to the outer function’s variable even after the outer function returns

One of the most important and ticklish features with closures is that the inner function still has access to the outer function’s variables even after the outer function has returned. This means that even after the outer function has returned, the inner function still has access to the outer function’s variables. Therefore, you can call the inner function later in your program.

```js
function celebrityName(firstName) {
  var nameIntro = "This celebrity is ";
  // this inner function has access to the outer function's variables,
  // including the parameter
  function lastName(theLastName) {
    return nameIntro + firstName + " " + theLastName;
  }
  return lastName;
}
// At this juncture, the celebrityName outer function has returned.
var mjName = celebrityName("Michael");
// The closure (lastName) is called here after the outer function has returned above
// Yet, the closure still has access to the outer function's variables and parameter
mjName("Jackson"); // This celebrity is Michael Jackson
```

### [4] Inner functions store their outer function’s variables by reference, not by value

**All **_**inner**_** functions **_**share**_** the **_**same parent**_** scope.** [[scope]] or parent context is shared among all inner functions. So if any change to parent's variable in any inner function reflect the change in other inner function too.

They do not copy the actual value. Closures get more interesting when the value of the outer function’s variable changes before the closure is called. And this powerful feature can be harnessed in creative ways, such as this private variables example first demonstrated by Douglas Crockford:

```js
function celebrityID() {
  var celebrityID = 999;
  // We are returning an object with some inner functions
  // All the inner functions have access to the outer function's variables
  return {
    getID: function() {
      // This inner function will return the UPDATED celebrityID variable
      // It will return the current value of celebrityID, even after the
      // changeTheID function changes it
      return celebrityID;
    },
    setID: function(theNewID) {
      // This inner function will change the outer function's variable anytime
      celebrityID = theNewID;
    }
  }
}
var mjID = celebrityID(); // At this juncture, the celebrityID outer function has returned.
mjID.getID(); // 999
mjID.setID(567); // Changes the outer function's variable
mjID.getID(); // 567: It returns the updated celebrityId variable
```

### [5] Closures Gone Awry** (away from the usual or expected course)

Because closures have access to the updated values of the outer function’s variables, they can also lead to bugs when the outer function’s variable changes with a for loop. Thus:

```js
var data = [];

for (var k = 0; k < 3; k++) {
  data[k] = function () {
    console.log(k);
  };
}

data[0](); // 3, but not 0
data[1](); // 3, but not 1
data[2](); // 3, but not 2
```

The reason this happened was because, as we have discussed in the previous example, the closure (the anonymous function in this example) has access to the outer function’s variables by reference, not by value. Accordingly, at the moment of function activations or called, last assigned value of `k` variable, i.e. `3` is used.

**Solution** - Execute the function when declared, using self-executed function.

```js
var data = [];

for (var k = 0; k < 3; k++) {
  data[k] = (function _helper(x) {
    return function () {
      console.log(x);
    };
  })(k); // pass "k" value
}

// now it is correct
data[0](); // 0
data[1](); // 1
data[2](); // 2

/**
First, the function _helper is created and immediately activated with the argument k.

Then, returned value of the _helper function is also a function, and exactly it is saved to
the corresponding element of the data array.

This technique provides the following effect: being activated, the _helper every time
creates a new activation object which has argument x, and the value of this argument is
the passed value of k variable.
**/
```

> **It is important to take note** that closures cannot access the outer function’s _this_ variable by using the _this_ keyword because the _this_ variable is accessible only by the function itself, not by inner functions.

## Callback

A callback function, also known as a higher-order function, is a function that is passed to another function (let’s call this other function “otherFunction”) as a parameter, and the callback function is called (or executed) inside the otherFunction. A callback function is essentially a pattern (an established solution to a common problem), and therefore, the use of a callback function is also known as a callback pattern.

**Callbacks and closures.** As we know, closures have access to the containing function's scope, so the callback function can access the containing functions' variables, and even the variables from the global scope. JavaScript's support for closures allow you to register callbacks that, when executed, maintain access to the environment in which they were created even though the execution of the callback creates a new call stack entirely (execute somewhere else but sill maintain the state where they are defined). Callback almost magically -- remember the context in which it was declared, along with all the variables available in that context and any parent contexts. Consider the following example:

```js
function changeHeaderDeferred() {
  var header = document.getElementById("header");

  setTimeout(function changeHeader() {
    header.style.color = "red";

    return false;
  }, 100);

  return false;
}

changeHeaderDeferred();
```

In this example, the `changeHeaderDeferred` function is executed which includes variable header. The function `setTimeout` is invoked, which causes a message (plus the `changeHeader` callback) to be added to the message queue approximately 100 milliseconds in the future. The `changeHeaderDeferred` function then returns `false`, ending the processing of the first message – but the header variable is still referenced via a closure and is not garbage collected. When the second message is processed (the `changeHeader` function) it maintains access to the header variable declared in the outer function's scope. Once the second message (the `changeHeader` function) is processed, the header variable can be garbage collected.
