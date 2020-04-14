# Object

## Getting and setting prototypes

### Object.create()

> Object.create([object to use as a prototype], [properties of new object])

```js
var someObject = {
  x: 1,
  y: 2
};
var newObject = Object.create(someObject, {
  size: {
    value: 'large'
  },
  shape: {
    value: 'round',
  }
});

console.dir(newObject);

/*
newObject
  shape: "round"
  size: "large"
  __proto__: someObject
    x: 1
    y: 2
*/
```

### Object.getPrototypeOf()

```js
var someObject = {
  x: 1,
  y: 2
};
var newObject = Object.create(someObject, {
  size: {
    value: 'large'
  },
  shape: {
    value: 'round',
  }
});

console.dir(Object.getPrototypeOf(newObject));

/*
someObject
   x: 1
   y: 2
*/
```

### Managing property attributes via property descriptors

There are two points to keep in mind:

1. When object property is created without using `Object.defineProperty()` , for example `obj.x = 1` or `var obj = {x: 1}` then by default the property is set to enumerable === `true`.
2. When object property is created by using `Object.defineProperty()` and you do not set the enumerable property then by default js set it to `false`.

#### Object.defineProperty()

Defines new or modifies existing property directly on an object, returning the object.

* **Configurable : default = false**,

  Can i use `defineProperty` method again to update the object's configuration such as Enumerable, Writable, get and set?

  You can update the previous behaviors of the properties if they are defined as configurable.

  1. If configurable = false then If the property is writable, you can convert it to non-writable. Any other try of definition update will fail throwing a `TypeError`.
  2. If configurable = false then properties can't be removed from the object using the operator `delete`.

* **Enumerable : default = false** I can access to all of them using a `for..in` loop. Also, enumerable property keys of an object are returned using [`Object.keys`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) method and they are not serialized when using `JSON.stringify`

* **Writable : default = false** I can modify their values, I can update a property just assigning a new value to it: `ob.a = 1000`;
* **get : default = undefined**
* **set : default = undefined**

```js
// Create a user-defined object.
var obj = {};

// Add a data property to the object.
Object.defineProperty(obj, "newDataProperty", {
  value: 101,
  writable: true,
  enumerable: true,
  configurable: true
});

console.log(obj.newDataProperty) // 101
```

```js
// Configurable Example

var ob = {};
Object.defineProperty( ob, 'a', {configurable:false, writable:true} );

Object.defineProperty(ob, 'a', { enumerable: true }); // throws a TypeError
Object.defineProperty(ob, 'a', { value: 12 }); // throws a TypeError
Object.defineProperty(ob, 'a', { writable: false }); // This is allowed!!
Object.defineProperty(ob, 'a', { writable: true }); // throws a TypeError
```

```js
// Enumerable Example

var ob = {a:1, b:2};

ob.c = 3;
Object.defineProperty(ob, 'd', {
  value: 4,
  enumerable: false
});

ob.d; // => 4

for( var key in ob ) console.log( ob[key] );
// Console will print out
// 1
// 2
// 3

Object.keys( ob );  // => ["a", "b", "c"]
JSON.stringify( ob ); // => "{a:1,b:2,c:3}"
ob.d; // => 4
```

```js
// Writable Example

var ob = {a: 1};

Object.defineProperty( ob, 'B', {value: 2, writable:false} );

ob.B; // => 2
ob.B = 10;
ob.B; // => 2
```

```js
// getter and setter example

/**
If you have a person object and you want to get fullname from first name and last name.
For that you will create an function to do that.
**/

var person = {
    firstName: 'Jimmy',
    lastName: 'Smith',
    fullName: function() {
        return this.firstName + ' ' + this.lastName;
    }
}

person.fullName(); // Jimmy Smith

// In ES5, we have getter and setter property of an object
var person = {
    firstName: 'Jimmy',
    lastName: 'Smith',
    get fullName() {
        return this.firstName + ' ' + this.lastName;
    },
    set fullName (name) {
        var words = name.toString().split(' ');
        this.firstName = words[0] || '';
        this.lastName = words[1] || '';
    }
}

// get in action
console.log(person.fullName); // Jimmy Smith

// set in action, setting person's object first name and last name value
person.fullName = 'Jack Franklin'; // settting value
console.log(person.fullName); // Jack Franklin

/*** The official way: Object.defineProperty ***/

var person = {
    firstName: 'Jimmy',
    lastName: 'Smith'
};

Object.defineProperty(person, 'fullName', {
    get: function() {
        return firstName + ' ' + lastName;
    },
    set: function(name) {
        var words = name.split(' ');
        this.firstName = words[0] || '';
        this.lastName = words[1] || '';
    }
});
```

#### Object.defineProperties()

Defines new or modifies existing properties directly on an object, returning the object.

```js
var obj = {};
Object.defineProperties(obj, {
  "property1": {
    value: true,
    writable: true
  },
  "property2": {
    value: "Hello",
    writable: false
  }
  // etc. etc.
});
```

#### Object.getOwnPropertyDescriptor()

```js
// Create a user-defined object.
var obj = {};

// Add a data property.
obj.newDataProperty = "abc";

// Get the property descriptor.
var descriptor = Object.getOwnPropertyDescriptor(obj, "newDataProperty");
console.log(descriptor); // Object {value: "abc", writable: true, enumerable: true, configurable: true}

// Change a property attribute.
descriptor.writable = false;
Object.defineProperty(obj, "newDataProperty", descriptor);
console.log(descriptor); // Object {value: "abc", writable: false, enumerable: true, configurable: true}
```

### Listing properties

#### Object.keys()

```js
// array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// object
var obj = {
  0: 'a',
  1: 'b',
  2: 'c'
};
console.log(Object.keys(obj)); // console: ['0', '1', '2']
```

#### Object.getOwnPropertyNames()

Returns the names of the own properties of an object. The own properties of an object are those that are defined directly on that object, and are not inherited from the object's prototype. The properties of an object include both fields (objects) and functions.

```js
function Pasta(grain, width, shape) {
    // Define properties.
    this.grain = grain;
    this.width = width;
    this.shape = shape;
    this.toString = function () {
        return (this.grain + ", " + this.width + ", " + this.shape);
    }
}

// Create an object.
var spaghetti = new Pasta("wheat", 0.2, "circle");

// Get the own property names.
var arr = Object.getOwnPropertyNames(spaghetti);
console.log(arr); // grain,width,shape,toString
```

> The `getOwnPropertyNames` method returns the names of both enumerable and non-enumerable properties and methods. To return only the names of enumerable properties and methods, you can use the `Object.keys` Function.

### Protecting object - Making immutable object

#### Object.preventExtensions()

Prevents new properties from ever being added to an object.

```js
var obj = { pasta: "spaghetti", length: 10 };
// Make the object non-extensible.
Object.preventExtensions(obj);
// Try to add a new property, and then verify that it is not added.
obj.newProp = 50;
console(obj.newProp); // undefined
```

#### Object.isExtensible()

Check if new property can be added to object.

#### Object.seal()

Prevents the modification of attributes of existing properties, and prevents the addition of new properties.

```js
// Create an object that has two properties.
var obj = {
  pasta: "spaghetti",
  length: 10
};
// Seal the object.
Object.seal(obj);

// Try to add a new property, and then verify that it is not added.
obj.newProp = 50;
console.log(obj.newProp); // undefined

// Try to delete a property, and then verify that it is still present.
delete obj.length;
console.log(obj.length); // 10
```

#### Object.isSealed()

Check if object is sealed or not.

#### Object.freeze()

Prevents the modification of existing property attributes and values, and prevents the addition of new properties.

**What is the difference between `seal()` and `freeze()`**

In Freeze **configurable , enumerable and writable** attributes of the object are set to `false`. where as in Sealed **writable** attribute is set to `true` and rest of the attributes are false.

**Freeze** = cannot do add/edit/delete

**Seal** = cannot do add/delete but edit.

#### Object.isFrozen()

Check if object is freeze or not.
