# Classes

Unlike most formal object-oriented programming languages, JavaScript didn't support classes and classical inheritance as the primary way of defining similar and related objects when it was created. Many libraries created utilities to make JavaScript look like it supports classes. While some JavaScript developers do feel strongly that the language doesn't need classes, the number of libraries created specifically for this purpose led to the inclusion of classes in ECMAScript 6.

**ECMAScript 6 classes aren't exactly the same as classes in other languages.**

```js
// ### ECMAScript 5

// The closest equivalent to a class was creating a constructor
// and then assigning methods to the constructor’s prototype, an approach
// typically called creating a custom type.

function PersonType(name) {
    this.name = name;
}

PersonType.prototype.sayName = function() {
    console.log(this.name);
};

let person = new PersonType("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonType);  // true
console.log(person instanceof Object);      // true
```

```js
// ### Class Declaration

class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
}

let person = new PersonClass("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"

/**
1. The class declaration `PersonClass` behaves quite similarly to `PersonType`
   from the previous example. But instead of defining a function as the
   constructor, class declarations allow you to define the
   constructor directly inside the class with the special constructor method name.

2. there’s no need to use the `function` keyword.

3. Interestingly, the `PersonClass` declaration actually creates a function
   that has the behavior of the constructor method, which is why
  `typeof PersonClass` gives "function" as the result. The `sayName()`
   method also ends up as a method on `PersonClass.prototype` in this example,
   similar to the relationship between `sayName()` and `PersonType.prototype`
   in the previous example.

4. "Own properties", properties that occur on the instance rather than attached to
    the prototype, can only be created inside a class constructor or method.
**/
```

## Why use class syntax

Despite the similarities between classes and custom types by function constructor, there are some important differences to keep in mind:

1. Class declarations, unlike function declarations, are not hoisted. Class declarations act like `let` declarations and so exist in the temporal dead zone until execution reaches the declaration. An important difference between **function declarations** and **class declarations** is that function declarations are [hoisted](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) and class declarations are not.
2. All code inside of class declarations runs in strict mode automatically. There's no way to opt-out of strict mode inside of classes.
3. All methods are non-enumerable. This is a significant change from custom types, where you need to use `Object.defineProperty()`to make a method non-enumerable.
4. All methods lack an internal `[[Construct]]` method and will throw an error if you try to call them with `new`.
5. Calling the class constructor without `new` throws an error.
6. Attempting to overwrite the class name within a class method throws an error.

```js
// With all of this in mind, the PersonClass declaration from the
// previous example is directly equivalent to the following code,
// which doesn’t use the class syntax:

// direct equivalent of PersonClass
let PersonType2 = (function() {

    "use strict";

    const PersonType2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.name = name;
    }

    Object.defineProperty(PersonType2.prototype, "sayName", {
        value: function() {

            // make sure the method wasn't called with new
            if (typeof new.target !== "undefined") {
                throw new Error("Method cannot be called with new.");
            }

            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonType2;
}());
```

## Ways to write class

```js
// ### 1. Declaration
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
}

// ### 2. Anonymous Expression
let PersonClass = class {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

// ### 3. Named Expression
let PersonClass = class PersonClass2 {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

console.log(typeof PersonClass);        // "function"
console.log(typeof PersonClass2);       // "undefined"

// In this example, the class expression is named PersonClass2. The PersonClass2 identifier
// exists only within the class definition so that it can be used inside the class methods
// (such as the sayName() method in this example). Outside the class, typeof PersonClass2 is
// "undefined" because no PersonClass2 binding exists there. To understand why this is, look
// at an equivalent declaration that doesn’t use classes:

// direct equivalent of PersonClass named class expression
let PersonClass = (function() {

    "use strict";

    const PersonClass2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.name = name;
    }

    Object.defineProperty(PersonClass2.prototype, "sayName", {
        value: function() {

            // make sure the method wasn't called with new
            if (typeof new.target !== "undefined") {
                throw new Error("Method cannot be called with new.");
            }

            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonClass2;
}());
```

> Whether you use class declarations or class expressions is mostly a matter of style. Unlike function declarations and function expressions, both class declarations and class expressions are not hoisted, and so the choice has little bearing on the runtime behavior of the code. The only significant difference is that anonymous class expressions have a `name`property that is an empty string while class declarations always have a `name` property equal to the class name (for instance, `PersonClass.name` is `"PersonClass"` when using a class declaration).

## Classes as first-class citizens

In programming, something is said to be a _first-class citizen_ when it can be used as a value, meaning it can be passed into a function, returned from a function, and assigned to a variable. JavaScript functions are first-class citizens.

ECMAScript 6 continues this tradition by making classes first-class citizens as well. That allows classes to be used in a lot of different ways. For example, they can be passed into functions as arguments:

```js
function createObject(classDef) {
    return new classDef();
}

let obj = createObject(class {

    sayHi() {
        console.log("Hi!");
    }
});

obj.sayHi();        // "Hi!"

// The createObject() function is called with an anonymous class expression
// as an argument, creates an instance of that class with new, and returns
// the instance. The variable obj then stores the returned instance.

// Another interesting use of class expressions is creating singletons
// by immediately invoking the class constructor. To do so, you must
// use new with a class expression and include parentheses at the end.

let person = new class {

    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }

}("Nicholas");

person.sayName();       // "Nicholas"
```

## Accessor properties

While own properties should be created inside class constructors, classes allow you to define accessor properties on the prototype. To create a getter, use the keyword `get` followed by a space, followed by an identifier; to create a setter, do the same using the keyword `set`.

```js
class CustomHTMLElement {

    constructor(element) {
        this.element = element;
    }

    get html() {
        return this.element.innerHTML;
    }

    set html(value) {
        this.element.innerHTML = value;
    }
}

var descriptor = Object.getOwnPropertyDescriptor(CustomHTMLElement.prototype, "html");
console.log("get" in descriptor);   // true
console.log("set" in descriptor);   // true
console.log(descriptor.enumerable); // false

// ### Other way to write above example using Function Constructor

// direct equivalent to previous example
let CustomHTMLElement = (function() {

    "use strict";

    const CustomHTMLElement = function(element) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.element = element;
    }

    Object.defineProperty(CustomHTMLElement.prototype, "html", {
        enumerable: false,
        configurable: true,
        get: function() {
            return this.element.innerHTML;
        },
        set: function(value) {
            this.element.innerHTML = value;
        }
    });

    return CustomHTMLElement;
}());
```

## Computed/Dynamic member names

Instead of using an identifier, use square brackets around an expression, which is the same syntax used for object literal computed names.

```js
let methodName = "sayName";

class PersonClass {

    constructor(name) {
        this.name = name;
    }

    [methodName]() {
        console.log(this.name);
    }
}

let me = new PersonClass("Nicholas");
me.sayName();           // "Nicholas"

// ### Accessor properties can use computed names in the same way, like this:

let propertyName = "html";

class CustomHTMLElement {

    constructor(element) {
        this.element = element;
    }

    get [propertyName]() {
        return this.element.innerHTML;
    }

    set [propertyName](value) {
        this.element.innerHTML = value;
    }
}
```

## Static members

Static methods are called without instantiating their class with new and are also not callable when the class is instantiated. Static methods are often used to create utility functions for an application.

```js
// ### ECMAScript 5
//Adding methods directly onto function's constructors to simulate static members.
function PersonType(name) {
    this.name = name;
}

// static method
PersonType.create = function(name) {
    return new PersonType(name);
};

// instance method
PersonType.prototype.sayName = function() {
    console.log(this.name);
};

var person = PersonType.create("Nicholas");

// ### ECMAScript 6
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }

    // equivalent of PersonType.create
    static create(name) {
        return new PersonClass(name);
    }
}

let person = PersonClass.create("Nicholas");
```

> Static members are not accessible from instances. You must always access static members from the class directly.

## Inheritance

ECMAScript 5, implementing inheritance with custom types was an extensive process. Proper inheritance required multiple steps. For instance, consider this example:

```js
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

function Square(length) {
    Rectangle.call(this, length, length);
}

Square.prototype = Object.create(Rectangle.prototype, {
    constructor: {
        value:Square,
        enumerable: true,
        writable: true,
        configurable: true
    }
});

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

Here's the ECMAScript 6 equivalent of the previous example:

```js
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true

/**
1. Classes that inherit from other classes are referred to as derived
   classes. Derived classes require you to use super() if you specify a
   constructor; if you don’t, an error will occur.

2. If you choose not to use a constructor, then super() is automatically
   called for you with all arguments upon creating a new instance of the class.

3. If there is a constructor present in sub-class, it needs to first call
   super() before using "this".

4. You can only use super() in a derived class. If you try to use it
   in a non-derived class (a class that doesn’t use extends) or a function,
  it will throw an error.
**/
```

## Shadowing/Overidding class methods

```js
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override and shadow Rectangle.prototype.getArea()
    getArea() {
        return this.length * this.length;
    }
}

// Since getArea() is defined as part of Square, the Rectangle.prototype.getArea()
// method will no longer be called by any instances of Square.
```

## Inherited static members

If a base class has static members, then those static members are also available on the derived class.

```js
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }

    static create(length, width) {
        return new Rectangle(length, width);
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var rect = Square.create(3, 4);

console.log(rect instanceof Rectangle);     // true
console.log(rect.getArea());                // 12
console.log(rect instanceof Square);        // false
```

## Derived classes from function expressions

```js
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x instanceof Rectangle);    // true
```

## Abstract Class - using new.target in class constructor

In the simple case, `new.target` is equal to the constructor function for the class.

```js
class Rectangle {
    constructor(length, width) {
        console.log(new.target === Rectangle);
        this.length = length;
        this.width = width;
    }
}

// new.target is Rectangle
var obj = new Rectangle(3, 4);      // outputs true

// ### Creating Abstract Base class
// abstract base class
class Shape {
    constructor() {
        if (new.target === Shape) {
            throw new Error("This class cannot be instantiated directly.")
        }
    }
}

class Rectangle extends Shape {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

var x = new Shape();                // throws error

var y = new Rectangle(3, 4);        // no error
console.log(y instanceof Shape);    // true
```
