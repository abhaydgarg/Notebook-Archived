# this

> `this` === Context

`this` (aka "the context") is a special keyword inside each function and its value only depends on _how_ the function was called, not when/where it was defined. It is not affected by lexical scope, like other variables.

```js
var person = {
    firstName: "Penelope",
    lastName: "Barrymore",
    fullName: function () {
        // Notice we use "this" just as we used "he" in the example sentence earlier?:
        console.log(this.firstName + " " + this.lastName);
       // We could have also written this:
        console.log(person.firstName + " " + person.lastName);
    }
}
```

If we use `person.firstName` and `person.lastName`, as in the example, our code becomes ambiguous. Consider that there could be another global variable (that we might or might not be aware of) with the name “person.” Then, references to `person.firstName` could attempt to access the firstName property from the person global variable, and this could lead to difficult-to-debug errors. So we use the “this” keyword not only for aesthetics (i.e., as a referent), but also for precision; its use actually makes our code more unambiguous.

## One principle to understand the `this` keyword

_this_ is not assigned a value until an object invokes the function where _this_ is defined.

In detail, Let’s call the function where _this_ is defined the `this Function`.

Even though it appears `this` refers to the object where it is defined, it is not until an object invokes the _this Function_ that _this_ is actually assigned a value. And the value it is assigned is based **exclusively** on the **object** that invokes the _this Function_. `this` has the value of the invoking object in most circumstances.

However, there are a few scenarios where _this_ does not have the value of the invoking object.

```js
var person = {
  firstName: "Penelope",
  lastName: "Barrymore",
  // Since the "this" keyword is used inside the showFullName method below, and the showFullName
  // method is defined on the person object,
  // "this" will have the value of the person object because the person object will invoke showFullName ()
  showFullName: function() {
    console.log(this.firstName + " " + this.lastName);
  }

}

person.showFullName(); // Penelope Barrymore
```

```js
$("button").click(function(event) {
  // $(this) will have the value of the button ($("button")) object
  // because the button object invokes the click () method
  console.log($(this).prop("name"));
});
```

## `this` in strict mode and global scope

### [1] In global scope

`this` always refer to `window` object in browser irrespective of strict mode. In the global scope, when the code is executing in the browser, all global variables and functions are defined on the `window` object. Therefore, when we use `this` in a global function, it refers to (and has the value of) the global `window` object.

```js
"use strict";
console.log(this); // [window] object

// ## OR ##

// "use strict"; // without strict mode
console.log(this); // [window] object
```

### [2] In strict mode

When function, even anonymous function is in global scope then the value of `this` will be `undefined`.

```js
function fn(){
  "use strict";
  console.log(this);
}
fn(); // undefined
```

### [3] In non strict mode

When function, even anonymous function is in global scope then the value of `this` will be `window` object in browser.

```js
function fn(){
  console.log(this);
}
fn(); // [window] object
```

## A bit about “Context”

The object that invokes the _this Function_ is a context, and we can change the context by invoking the _this Function_ with another object; then this new object is a _context_..

```js
var person = {
  firstName: "Penelope",
  lastName: "Barrymore",
  showFullName: function() {
    console.log(this.firstName + " " + this.lastName);
  }
}
// The "context", when invoking showFullName, is the person object,
// when we invoke the showFullName () method on the person object.
// And the use of "this" inside the showFullName() method has the
// value of the person object.
person.showFullName(); // Penelope Barrymore

// If we invoke showFullName with a different object:
var anotherPerson = {
  firstName: "Rohit",
  lastName: "Khan"
};
// We can use the apply method to set the "this" value explicitly—more
// on the apply () method later. "this" gets the value of whichever
// object invokes the "this" Function, hence:
person.showFullName.apply(anotherPerson); // Rohit Khan
// So the context is now anotherPerson because anotherPerson invoked
// the person.showFullName () method by virtue of using the
// apply () method
```

## Scenarios when the `this` keyword becomes tricky

### [1] Fix `this` when used in a method passed as a callback

```js
// We have a simple object with a clickHandler method that we want to use when a
// button on the page is clicked
var user = {
  data: [{
      name: "T. Woods",
      age: 37
    },
    {
      name: "P. Mickelson",
      age: 43
    }
  ],
  clickHandler: function(event) {
    var randomNum = ((Math.random() * 2 | 0) + 1) - 1; // random number between 0 and 1
    // This line is printing a random person's name and age from the data array
    console.log(this.data[randomNum].name + " " + this.data[randomNum].age);
  }
}
// The button is wrapped inside a jQuery $ wrapper, so it is now a jQuery object
// And the output will be undefined because there is no data property on the button object
$("button").click(user.clickHandler); // Cannot read property '0' of undefined
```

In the code above, since the button `($(“button”))` is an object on its own, and we are passing the `user.clickHandler` method to its `click()` method as a callback, we know that _this_ inside our `user.clickHandler` method will no longer refer to the _user_ object. `this` will now refer to the object where the `user.clickHandler` method is executed because `this` is defined inside the `user.clickHandler` method. And the object that is invoking `user.clickHandler` is the button object—`user.clickHandler` will be executed inside the button object’s click method.

Note that even though we are calling the `clickHandler ()` method with `user.clickHandler` (which we have to do, since clickHandler is a method defined on user), the `clickHandler ()` method itself will be executed with the button object as the context to which “this” now refers. So _this_ now refers to is the button `($(“button”))` object.

At this point, it should be apparent that when the context changes—when we execute a method on some other object than where the object was originally defined, the `this` keyword no longer refers to the original object where “this” was originally defined, but it now refers to the object that invokes the **method** where `this` was defined.

**Solution to fix this when a method is passed as a callback function:**
Since we really want this.data to refer to the data property on the _user_ object, we can use the `bind ()`, `apply ()`, or `call ()` method to specifically set the value of `this`.

```js
// # Instead of this line
$ ("button").click (user.clickHandler);

// # We have to bind the clickHandler method to the user object like this
$("button").click (user.clickHandler.bind (user)); // P. Mickelson 43
```

### [2] Fix `this` inside closure

It is important to take note that closures cannot access the outer function’s `this` variable by using the `this` keyword because the `this` variable is accessible only by the function itself, not by inner functions. For example:

```js
var user = {
  tournament: "The Masters",
  data: [{
      name: "T. Woods",
      age: 37
    },
    {
      name: "P. Mickelson",
      age: 43
    }
  ],

  clickHandler: function() {
    // the use of this.data here is fine, because "this" refers to the user object, and
    // data is a property on the user object.

    this.data.forEach(function(person) {
      // But here inside the anonymous function (that we pass to the forEach method), "this" no
      // longer refers to the user object.
      // This inner function cannot access the outer function's "this"

      console.log("What is This referring to? " + this); //[object Window]

      console.log(person.name + " is playing at " + this.tournament);
      // T. Woods is playing at undefined
      // P. Mickelson is playing at undefined
    })
  }
}
user.clickHandler(); // What is "this" referring to? [object Window]
```

`this` inside the anonymous function cannot access the outer function’s `this`, so it is bound to the global window object, when _strict_ mode is not being used, in strict mode it will be `undefined`.

**Solution to maintain this inside anonymous functions:**
To fix the problem with using _this_ inside the anonymous function passed to the _forEach_ method, we use a common practice in JavaScript and set the _this_ value to another variable before we enter the forEach method:

```js
var user = {
  tournament: "The Masters",
  data: [{
      name: "T. Woods",
      age: 37
    },
    {
      name: "P. Mickelson",
      age: 43
    }
  ],
  clickHandler: function(event) {
    // To capture the value of "this" when it refers to the user object, we have to set it to another variable here:
    // We set the value of "this" to theUserObj variable, so we can use it later
    var theUserObj = this;
    this.data.forEach(function(person) {
      // Instead of using this.tournament, we now use theUserObj.tournament
      console.log(person.name + " is playing at " + theUserObj.tournament);
    })
  }
}
user.clickHandler();
// T. Woods is playing at The Masters
// P. Mickelson is playing at The Masters
```

### [3] Fix `this` when method is assigned to a variable

The `this` value escapes our imagination and is bound to another object, if we assign a method that uses `this` to a variable. Let’s see how:

```js
// This data variable is a global variable
var data = [{
    name: "Samantha",
    age: 12
  },
  {
    name: "Alexis",
    age: 14
  }
];
var user = {
  // this data variable is a property on the user object
  data: [{
      name: "T. Woods",
      age: 37
    },
    {
      name: "P. Mickelson",
      age: 43
    }
  ],
  showData: function(event) {
    var randomNum = ((Math.random() * 2 | 0) + 1) - 1; // random number between 0 and 1
    // This line is adding a random person from the data array to the text field
    console.log(this.data[randomNum].name + " " + this.data[randomNum].age);
  }
}
// Assign the user.showData to a variable
var showUserData = user.showData;
// When we execute the showUserData function, the values printed to the console
// are from the global data array, not from the data array in the user object
showUserData();
// Samantha 12 (from the global data array) becaue
// showUserData() === window.showUserDate() and in this case window object is
// executing "this function"
```

**Solution for maintaining this when method is assigned to a variable:**
We can fix this problem by specifically setting the _this_ value with the bind method:

```js
// Bind the showData method to the user object
var showUserData = user.showData.bind (user);
// Now we get the value from the user object, because the
// this keyword is bound to the user object
showUserData (); // P. Mickelson 43
```
