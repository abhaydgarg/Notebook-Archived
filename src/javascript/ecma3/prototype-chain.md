# Prototype chain

ECMAScript, being a highly-abstracted object-oriented language, deals with *objects*. There are also *primitives*, but they, when needed, are also converted to objects.

An object is a *collection of properties* and has a *single prototype object*. The prototype may be either an object or the `null` value.

Let’s take a basic example of an object. A prototype of an object is referenced by the internal `[[Prototype]]` property. However, in figures we will use `__<internal-property>__` underscore notation instead of the double brackets, particularly for the prototype object: `__proto__`.

```js
var foo = {
  x: 10,
  y: 20
};
```

we have the structure with two explicit *own* properties and one implicit `__proto__`property, which is the reference to the prototype of `foo`:

![basic-object.png](assets/basic-object.png)

## Prototype Chain

Prototype objects are also just simple objects and may have their own prototypes. If a prototype has a non-null reference to its prototype, and so on, this is called the *prototype chain*.

A prototype chain is a *finite* chain of objects which is used to implement *inheritance* and *shared properties*.

Consider the case when we have two objects which differ only in some small part and all the other part is the same for both objects. Obviously, for a good designed system, we would like to *reuse* that similar functionality/code without repeating it in every single object. In class-based systems, this *code reuse* stylistics is called the *class-based inheritance* — you put similar functionality into the class `A`, and provide classes `B` and `C` which inherit from `A` and have their own small additional changes.

ECMAScript has no concept of a class. However, a code reuse stylistics does not differ much (though, in some aspects it’s even more flexible than class-based) and achieved via the *prototype chain*. This kind of inheritance is called a *delegation based inheritance* (or, closer to ECMAScript, a *prototype based inheritance*).

Similarly like in the example with classes `A`, `B` and `C`, in ECMAScript you create objects: `a`, `b`, and `c`. Thus, object `a` stores this common part of both `b` and `c`objects. And `b` and `c` store just their own additional properties or methods.

```js
var a = {
  x: 10,
  calculate: function (z) {
    return this.x + this.y + z;
  }
};

var b = {
  y: 20,
  __proto__: a
};

var c = {
  y: 30,
  __proto__: a
};

// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80
```

We see that `b` and `c` have access to the `calculate` method which is defined in `a` object. And this is achieved exactly via this prototype chain.

![prototype-chain.png](assets/prototype-chain.png)

**The rule is simple:** if a property or a method is not found in the object itself (i.e. the object has no such an *own* property), then there is an attempt to find this property/method in the prototype chain. If the property is not found in the prototype, then a prototype of the prototype is considered, and so on, i.e. the whole prototype chain (absolutely the same is made in class-based inheritance, when resolving an inherited *method* — there we go through the *class chain*). The first found property/method with the same name is used. Thus, a found property is called *inherited* property. If the property is not found after the whole prototype chain lookup, then `undefined` value is returned.

>ES5 standardized an alternative way for prototype-based inheritance using `Object.create` function:
>
>```js
>var b = Object.create(a, {y: {value: 20}});
>var c = Object.create(a, {y: {value: 30}});
>```
>
>ES6 though standardizes the `__proto__`, and it can be used at initialization of objects.

## Constructor Function and Prottype Chain

Besides creation of objects by specified pattern, a *constructor* function does another useful thing — it *automatically sets a prototype object* for newly created objects. This prototype object is stored in the `ConstructorFunction.prototype` property.

E.g., we may rewrite previous example with `b` and `c` objects using a constructor function. Thus, the role of the object `a` (a prototype) `Foo.prototype` plays:

```js
// a constructor function
function Foo(y) {
  // which may create objects
  // by specified pattern: they have after
  // creation own "y" property
  this.y = y;
}

// also "Foo.prototype" stores reference
// to the prototype of newly created objects,
// so we may use it to define shared/inherited
// properties or methods, so the same as in
// previous example we have:

// inherited property "x"
Foo.prototype.x = 10;

// and inherited method "calculate"
Foo.prototype.calculate = function (z) {
  return this.x + this.y + z;
};

// now create our "b" and "c"
// objects using "pattern" Foo
var b = new Foo(20);
var c = new Foo(30);

// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80

// let's show that we reference
// properties we expect

console.log(

  b.__proto__ === Foo.prototype, // true
  c.__proto__ === Foo.prototype, // true

  // also "Foo.prototype" automatically creates
  // a special property "constructor", which is a
  // reference to the constructor function itself;
  // instances "b" and "c" may found it via
  // delegation and use to check their constructor

  b.constructor === Foo, // true
  c.constructor === Foo, // true
  Foo.prototype.constructor === Foo, // true

  b.calculate === b.__proto__.calculate, // true
  b.__proto__.calculate === Foo.prototype.calculate // true

);
```

![constructor-proto-chain.png](assets/constructor-proto-chain.png)

This figure again shows that every object has a prototype. Constructor function `Foo`also has its own `__proto__` which is `Function.prototype`, and which in turn also references via its `__proto__` property again to the `Object.prototype`. Thus, repeat, `Foo.prototype` is just an explicit property of `Foo` which refers to the prototype of `b`and `c` objects.

> ES6 the concept of a “class” is standardized, and is implemented as exactly a syntactic sugar on top of the constructor functions as described above.

```js
// ES6
class Foo {
  constructor(name) {
    this._name = name;
  }

  getName() {
    return this._name;
  }
}

class Bar extends Foo {
  getName() {
    return super.getName() + ' Doe';
  }
}

var bar = new Bar('John');
console.log(bar.getName()); // John Doe
```
