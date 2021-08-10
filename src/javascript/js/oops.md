# OOPS

## Objects

Object can be thought of as the main actors in an application, or simply the main "things" or building blocks that do all the work. As you know by now, objects are everywhere in JavaScript since every component in JavaScript is an Object, including Functions, Strings, and Numbers. We normally use object literals or constructor functions to create objects.

## Encapsulation and inheritance

**Encapsulation** refers to enclosing all the functionalities of an object within that object so that the object's internal workings (its methods and properties) are hidden from the rest of the application. This allows us to abstract or localize specific set of functionalities on objects.

**Inheritance** refers to an object being able to inherit methods and properties from a parent object.

**JavaScript supports single inheritance only.**

### Object creation (Encapsulation)

Which is done by following patterns.

#### [1] Object Literals Pattern

For simple objects that used in your application to store data.

```js
//This is an object with 4 items, using object literal

var mango = {
  color: "yellow",
  shape: "round",
  sweetness: 8,
  howSweetAmI: function () {
   // access property
   console.log(this.color);
   console.log("Hmm Hmm Good");
  }
};
```

#### [2] Function constructor pattern

```js
function Fruit (theColor, theSweetness, theFruitName) {

  this.color = theColor;
  this.sweetness = theSweetness;
  this.fruitName = theFruitName;

  this.showName = function () {
    // access property
    console.log("This is a " + this.fruitName);
  }

}
//With this pattern in place, it is very easy to create all sorts of fruits.
var mangoFruit = new Fruit ("Yellow", 8, "Mango");
mangoFruit.showName(); // This is a Mango.

var pineappleFruit = new Fruit ("Brown", 5, "Pineapple");
pineappleFruit.showName(); // This is a Pineapple.
```

##### Function constructor pattern with sharing mechanism

This makes object to share constructor function properties and methods. If one object make change to property called `color` the all other objects created from `Fruit` constructor function can see the change. `Prototype` object share among `Fruit` objects.

If you want all objects to share property and method then put it into `prototype` property.

```js
function Fruit () {
}

Fruit.prototype.color = "Yellow";
Fruit.prototype.sweetness = 7;
Fruit.prototype.fruitName = "Generic Fruit";

Fruit.prototype.showName = function () {
  console.log("This is a " + this.fruitName);
}

var mangoFruit = new Fruit ();
mangoFruit.showName();
```

> Best way to create object by Function Constructor pattern and incorporate prototype to add sharing feature in object if needed.

### Code reuse (Inheritance)

Which is done by object prototype pattern. But before we jump into various prototype pattern lets understand the prototype concept.

#### Prototype concept

There are two interrelated concepts with prototype in JavaScript:

##### 1. Prototype property

> Belong to function only.

First, every JavaScript function has a prototype property (this property is empty by default), and you attach properties and methods on this prototype property when you want to implement inheritance. This prototype property is not enumerable; that is, it isn't accessible in a for/in loop.

**The prototype property is used primarily for inheritance in Function Constructor**; you add methods and properties on a function's prototype property to make those methods and properties available to instances of that function. Besides creation of objects by specified pattern, a _constructor_ function does another useful thing — it _automatically sets a prototype object_ `__proto__` for newly created objects when using `new` keyword with function prototype property.

**Note:** When we talk about `prototype` property then it is in the context of Function Constructor, which is used to implement sharing mechanism among instances that are created from function constructor method.

##### 2. Prototype object `__proto__`

> Belongs to instances (object) created by `new` keyword.

The second concept with prototype in JavaScript is the prototype object. `__proto__` is the actual object that is used in the lookup chain to resolve identifiers. It is a property that all objects have by default. In simple terms: An object's prototype object points to the object's "parent prototype"-- the object it inherited its properties from. It is set automatically when new object is created.

**Step by step explanation of `prototype` property and `__proto__`**

```js
// Prototype property is created when a function is declared.
// `Person.prototype` property is created internally once you
// declare above function.
function Person(dob){
   this.dob = dob
};

//adds a new method age to the `Person.prototype` Object.
// `Person.prototype` is an Object literal by default. Many
// properties can be added to the `Person.prototype`
// which are shared by `Person` instances created using `new Person()`.
Person.prototype.age = function(){return date-dob};

// Every instance created using `new Person()` has a `__proto__`
// object attached internally by interpreter which points to the
// `Person.prototype`. This is the chain that is used to traverse
// to find a property of a particular object.

// this is when __proto__ object is attached to instance person1 and
// give value Person.prototype behind the scene.
var person1 = new Person(somedate);

//person1 object has `__proto__' object that point to its constructor
// function `prototype` property.
// that is: person1.__proto__ === Person.prototype
```

**`constructor` property, accessing `prototype` property and `__proto__`**

Constructor property is simply a property of an object that points to the constructor of the object. Javascript internally set it for every object. So, if we create an empty object then it is treated by interpreter as an empty object but still it has property called `constructor`. Constructor tells, **who has created this object.**

```js
var obj = {}; //empty object, but still has property called `constructor`.
obj.constructor // Object()


// defining function constructor
function C(){}

var o = new C();

o.constructor // points to C, object `o` is created from C() constructor function

//accessing __proto__ of object `o` which in turn points to C() constructor
// function's property called `prototype`.
Object.getPrototypeOf(o) // C.prototype
o.constructor.prototype // C.prototype
o.__proto__ // C.prototype
```

**Objects created with `new Object ()` or object literal `{}`**

All objects created with object literals and with the Object constructor inherits from `Object.prototype`.

```js
// The `userAccount` object and `userAccountOne` object created from
// `Object` therefore `__proto__` points to the Object.prototype.

var userAccount = new Object();
var userAccountOne = {};

userAccount.__proto__ // Object.prototype
userAccountOne.__proto__ // Object.prototype
```

**What happen when you change the **`prototype`** property of Function Constructor?**

```js
function Account() {}

// Account(), internally when `prototype` property is created for function then
// interpreter attach `constructor` property to `prototype` object.
Account.prototype.constructor

// replaced by `new Object`, this erase the `constructor` property already set for `prototype`
Account.prototype = {};

Account.prototype.constructor // Object()

// Also instances created from Account has constructor points to `Object()`
var userAccount = new Account();
console.log(userAccount.constructor); // points to `Object()`. It should point to `Account()`

// sometime we assign another object to prototype property for implementing inheritance.
// This makes constructor to point to the object which is assigned to prototype property.
// Check out "Pseudoclassical pattern" section where during inheritance we assign tempObj()
// to prototype property so that sharing can be done by copy by value mechanism rather
// than reference mechanism.

// to make constructor function unchanged just reassign it manually.

function Account() {}
Account.prototype = {}; //replaced by new Object
Account.prototype.constructor = Account;

var userAccount = new Account();
console.log(userAccount.constructor); // points to Account()
```

**Prototype Chain.**

Scan `__proto__` of all objects in chain. If a property or a method is not found in the object itself (i.e. the object has no such an own property), then there is an attempt to find this property/method in the `prototype object means __proto__` of object and scanned till interpreter reach `Object.prototype`, which is end of chain. In any stage when interpreter found property/method then it stops. If the property is not found in prototype chain then `undefined` value is returned.

For example: It is one way look to `__proto__`, interpreter does not jump to parent. It looks down down in the chain.

`BaseObjectInstance.__proto__.__proto__.__proto__.__proto__` ends when encounter `Object.prototype`

#### Inheritance patterns

##### [1] Pseudoclassical pattern

> with prototype, constructor function and new keyword

A pattern which uses a `constructor function` and the `new` operator, combined with a `prototype` added to the constructor is said to be Pseudoclassical.

```js
/**
 * Point a child's prototype to a parent's prototype
 **/
var extendObj = function(childObj, parentObj) {
    childObj.prototype = parentObj.prototype;
};

// base human object
var Human = function() {};
// inhertiable attributes / methods
Human.prototype = {
    name: '',
    gender: '',
    planetOfBirth: 'Earth',
    sayGender: function () {
        alert(this.name + ' says my gender is ' + this.gender);
    },
    sayPlanet: function () {
        alert(this.name + ' was born on ' + this.planetOfBirth);
    }
};

// male
var Male = function (name) {
    this.gender = 'Male';
    this.name = 'David';
};
// inherits human
extendObj(Male, Human);

// female
var Female = function (name) {
    this.name = name;
    this.gender = 'Female';
};
// inherits human
extendObj(Female, Human);

// new instances
var david = new Male('David');
var jane = new Female('Jane');

david.sayGender(); // David says my gender is Male
jane.sayGender(); // Jane says my gender is Female

Male.prototype.planetOfBirth = 'Mars';
david.sayPlanet(); // David was born on Mars
jane.sayPlanet(); // Jane was born on Mars
```

As expected, we have achieved inheritance in a Pseudoclassical manner, however, this solution has a problem. If you look at the last line, you will see the alert says `Jane was born on Mars`, but what we really want it to say is `Jane was born on Earth`. The reason for this is the Male prototype was changed to "Mars".

Given the direct link between the `Male` and `Human` prototype, if `Human` has many children inheriting from it, any change on a child's prototype properties will affect `Human`, and thus all children inheriting from `Human`. Changing a child's prototype should not affect other children inheriting from the same parent. The reason for this is **because JavaScript passes objects by reference, not by value**, meaning all children of Human inherit changes occurred on other children's prototypes.

`childObj.prototype = parentObj.prototype` does give us inheritance. However, if you want to fix the issue above, you need to replace the `extendObj` function to take the child's prototype and link it to a temporary object, whose prototype is the parent object's prototype. In this way, by creating a `temporary` "middle" object, you allow the temporary object to be empty and inherit its properties from Human.

By doing this, you have solved the pass by reference issue with a new instance of an empty object, which still inherits from the parent, but is not affected by other children.

```js
/**
 * Create a new constructor function, whose prototype is the parent object's prototype.
 * Set the child's prototype to the newly created constructor function.
 **/
var extendObj = function(childObj, parentObj) {
    var tmpObj = function () {}
    tmpObj.prototype = parentObj.prototype;
    childObj.prototype = new tmpObj(); // new keyword create copy in memory than reference
    childObj.prototype.constructor = childObj;
};

// base human object
var Human = function () {};
// inhertiable attributes / methods
Human.prototype = {
    name: '',
    gender: '',
    planetOfBirth: 'Earth',
    sayGender: function () {
        alert(this.name + ' says my gender is ' + this.gender);
    },
    sayPlanet: function () {
        alert(this.name + ' was born on ' + this.planetOfBirth);
    }
};

// male
var Male = function (name) {
    this.gender = 'Male';
    this.name = 'David';
};
// inherits human
extendObj(Male, Human);

// female
var Female = function (name) {
    this.name = name;
    this.gender = 'Female';
};
// inherits human
extendObj(Female, Human);

// new instances
var david = new Male('David');
var jane = new Female('Jane');

david.sayGender(); // David says my gender is Male
jane.sayGender(); // Jane says my gender is Female

Male.prototype.planetOfBirth = 'Mars';
david.sayPlanet(); // David was born on Mars
jane.sayPlanet(); // Jane was born on Earth
```

**Rewrite Pseudoclassical with `Object.create()`** - instead using `extendObj()` to extend superclass as done above.

```js
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass, not need to worry about doing new tempObj()
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

// ####### OR - in one statement than two as above #######
Rectangle.prototype = Object.create(Shape.prototype, {
  constructor: {
    value: Rectangle,
    enumerable: false,
    writable: true,
    configurable: true
  }
});

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle);// true
console.log('Is rect an instance of Shape?', rect instanceof Shape);// true
rect.move(1, 1); // Outputs, 'Shape moved.'
```

###### More about constructor

Every object has constructor property which tells who has created an object.

So how constructor property works and what happen during prototype inheritance.

```js
var P = function () {};
// interpreter creates an prototype object behind scene for
// constructor function P and set constructor === P
P.prototype.vip = "Parent var";

/***
IF you try to do:

P.prototype = {
  vip: "Parent var"
};

this overwrite constructor property, so instead point to P it now set to Object.
So in order to reset it to original one you must do

p.prototype.constructor = P;
***/

console.log('P => ' + P.prototype.constructor.name); // P

var C = function () {
};
// interpreter set C.prototype object constructor to C

console.log('C => ' + C.prototype.constructor.name); // C

C.prototype = P.prototype;
// line 25 change the C.prototype constructor to P
console.log('C => ' + C.prototype.constructor.name); // P
// reset C.prototype constructor to C
C.prototype.constructor = C;

console.log('C => ' + C.prototype.constructor.name); // C

var o = new C();

console.log(o.vip);
```

##### [2] Functional pattern

> without prototype, constructor function and new keyword

This pattern allows one object to inherit from another, take the result and augment it at the child level to achieve inheritance. What this really means, is you create an object as your parent, pass the child object to the parent to inherit / apply its properties, and return the resulting object back to the child, who can then augment its own properties to the object returned from the parent.

```js
var human = function(name) {
    var that = {};

    that.name = name || '';
    that.gender = '';
    that.planetOfBirth = 'Earth';
    that.sayGender = function () {
        alert(that.name + ' says my gender is ' + that.gender);
    };
    that.sayPlanet = function () {
        alert(that.name + ' was born on ' + that.planetOfBirth);
    };

    return that;
}

var male = function (name) {
    var that = human(name);
    that.gender = 'Male';
    return that;
}

var female = function (name) {
    var that = human(name);
    that.gender = 'Female';
    return that;
}

var david = male('David');
var jane = female('Jane');

david.sayGender(); // David says my gender is Male
jane.sayGender(); // Jane says my gender is Female

david.planetOfBirth = 'Mars';
david.sayPlanet(); // David was born on Mars
jane.sayPlanet(); // Jane was born on Earth
```

As you can see by using this pattern, there is no need to use the prototype chain, constructors or the "new" keyword. This however, has a downside for performance because each object is unique, meaning each function call creates a new object, so the JavaScript interpreter has to assign new memory to the function in order to recompile everything inside of it as unique again.

There are also benefits to this approach, as the closures of each function allow for good use of public and private methods / attributes. Let's take this code for example, which shows a parent class of vehicle and children classes of `motorbike` and `boat`.

```js
var vehicle = function(attrs) {
    var _privateObj = {
        hasEngine: true
    },
    that = {};

    that.name = attrs.name || null;
    that.engineSize = attrs.engineSize || null;
    that.hasEngine = function () {
        alert('This ' + that.name + ' has an engine: ' + _privateObj.hasEngine);
    };

    return that;
}

var motorbike = function () {

    // private
    var _privateObj = {
        numWheels: 2
    },

    // inherit
    that = vehicle({
        name: 'Motorbike',
        engineSize: 'Small'
    });

    // public
    that.totalNumWheels = function () {
        alert('This Motobike has ' + _privateObj.numWheels + ' wheels');
    };

    that.increaseWheels = function () {
        _privateObj.numWheels++;
    };

    return that;

};

var boat = function () {

    // inherit
    that = vehicle({
        name: 'Boat',
        engineSize: 'Large'
    });

    return that;

};

myBoat = boat();
myBoat.hasEngine(); // This Boat has an engine: true
alert(myBoat.engineSize); // Large

myMotorbike = motorbike();
myMotorbike.hasEngine(); // This Motorbike has an engine: true
myMotorbike.increaseWheels();
myMotorbike.totalNumWheels(); // This Motorbike has 3 wheels
alert(myMotorbike.engineSize); // Small

myMotorbike2 = motorbike();
myMotorbike2.totalNumWheels(); // This Motorbike has 2 wheels

myMotorbike._privateObj.numWheels = 0; // undefined
myBoat.totalNumWheels(); // undefined
```

You can see that it is fairly easy to provide encapsulation. The `_privateObj` can not be modified from outside of the object, unless exposed by a public method like `increaseWheels()`. Similarly, private values can also only be read when exposed by a public method, such as motorbike's `totalNumWheels()` function.
