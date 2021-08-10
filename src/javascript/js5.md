# Javascript 5

## Strict Mode

The "use strict" directive is only recognized at the beginning of a script or a function.

Strict mode is declared by adding `"use strict";` to the beginning of a JavaScript file, or a JavaScript function. Declared at the beginning of a JavaScript file, it has global scope (all code will execute in strict mode). Declared inside a function, it has local scope (only the code inside the function is in strict mode).

### Inheritence

```js
// define strict mode in the global context,
// i.e. for the whole program
"use strict";

(function foo() {
  // strictness is "inherited" from
  // the global context
  eval = 10; // SyntaxError
  (function bar() {
    // the same - from the global context
    arguments = 10; // SyntaxError
  })();
})();
```

### Restrictions

#### Reserved keywords

The following set of identifiers is classified as future reserved keywords and cannot be used as variable or function names: `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, and `yield`.

```js
// non-strict mode
var lеt = 10; // OK
console.log(lеt); // 10

"use strict";
var let = 10; // SyntaxError
```

#### Assignment to an undeclared identifier

As we know, in ES3 assignment to an undeclared identifier creates a new property of the global object. This causes also well-known issue related with unintentional polluting of the global scope.

```js
// non-strict mode

(function foo() {
  // local vars
  var x = y = 20;
})();

// unfortunately, "y" wasn't local
// for "foo" function
alert(y); // 20
alert(x); // "x" is not defined
```

In strict mode this feature has been removed:

```js
"use strict";
a = 10; // ReferenceError

var x = y = 20; // also a ReferenceError
```

#### Assignment to read-only properties

The same situation is with *read-only* properties, i.e. if a property being either a *data property* with `[[Writable]]` attribute set to `false` or an *accessor property**without* a `[[Set]]` method. In the following example a `TypeError` should be thrown:

```js
"use strict";

var foo = Object.defineProperties({}, {
  bar: {
    value: 10,
    writable: false // by default
  },
  baz: {
    get: function () {
      return "baz is read-only";
    }
  }
});

foo.bar = 20; // TypeError
foo.baz = 30; // TypeError
```

However, if a property is *configurable*, then we can change it via `Object.defineProperty`:

```js
"use strict";

var foo = Object.defineProperty({}, "bar", {
  value: 10,
  writable: false, // read-only
  configurable: true // but still configurable
});

// change the value
Object.defineProperty(foo, "bar", {
  value: 20
});

console.log(foo.bar); // OK, 20

// but still can't via assignment

foo.bar = 30; // TypeError
```

#### Shadowing inherited read-only properties

If we try to *shadow* some *read-only* inherited property via *assignment* we also get `TypeError`. And again, if we shadow the property via `Object.defineProperty` it’s made normally — thus, a `configurable` attribute of the inherited property doesn’t matter in this case:

```js
"use strict";

var foo = Object.defineProperty({}, "x", {
  value: 10,
  writable: false
});

// "bar" inherits from "foo"

var bar = Object.create(foo);

console.log(bar.x); // 10, inherited

// try to shadow "x" via assignment

bar.x = 20; // TypeError

console.log(bar.x); // still 10, if non-strict mode

// however shadowing works
// if we use "Object.defineProperty"

Object.defineProperty(bar, "x", { // OK
  value: 20
});

console.log(bar.x); // 20
```

#### Creating a new property of non-extensible objects

Restricted assignment also relates to augmenting non-extensible objects, i.e. objects having [[Extensible]] property as *false*:

```js
"use strict";

var foo = {};
foo.bar = 20; // OK

Object.preventExtensions(foo);
foo.baz = 30; // TypeError
```

**`eval` and `arguments` restrictions**

In strict mode these names — `eval` and `arguments` are treated as kind of “keywords” (while they are not) and not allowed in several cases.

They cannot appear as a variable declaration or as a function name:

```js
"use strict";

// SyntaxError in both cases
var arguments;
var eval;

// also SyntaxError
function eval() {}
var foo = function arguments() {};
```

They are not allowed as function argument names:

```js
"use strict";

// SyntaxError
function foo(eval, arguments) {}
```

Regarding objects and their properties, `eval` and `arguments` are *allowed* in assignment expressions and *may* be used as property names of objects:

```js
"use strict";

// OK
var foo = {
  eval: 10,
  arguments: 20
};

// OK
foo.eval = 10;
foo.arguments = 20;

```

However, `eval` and `arguments` are not allowed as parameter names of declarative view of a setter:

```js
"use strict";

// SyntaxError
var foo = {
  set bar (eval, arguments) {
    ...
  }
};
```

#### Duplications

Duplications of *data* property names in an object initialiser are restricted:

```js
"use strict";

// SyntaxError
var foo = {
  x: 10,
  x: 10
};
```

Duplications of formal parameters of functions are restricted as well:

```js
"use strict";

// SyntaxError
function foo(x, x) {}
```

Notice, that duplications of *accessor* properties (in case of duplicating the same operation, i.e. `get` or `set`) are restricted *regardless* the strict mode:

```js
// strict or non-strict mode

// SyntaxError
var foo = {
  get x() {},
  get x() {}
};

// the same with setters
// SyntaxError
var bar = {
  set y(value) {},
  set y(value) {}
};
```

Also duplications of mixed property types (i.e. both — a data and an accessor property with the same name) are restricted *regardless* the strict mode:

```js
// strict or non-strict mode

// SyntaxError
var foo = {
  x: 10,
  get x() {}
};
```

**`this` value restriction**

In the strict mode, a `this` value is not automatically coerced to an object. A `this` value is *not* converted to the *global object* and primitive values are not converted to wrapper objects. The `this` value passed via a function call (including calls made using `Function.prototype.apply` and `Function.prototype.call`) do not coerce the passed `this` value to an object:

```js
"use strict";

// undefined "this" value,
// but not the global object
function foo() {
  alert(this); // undefined
}

foo(); // undefined

// "this" is a primitive
Number.prototype.test = function () {
  alert(typeof this); // number
};

1..test(); // number

foo.call(null); // null
foo.apply(undefined); // undefined
```

`Undefined` value for `this` can help to avoid cases with using constructors, forgetting `new` keyword:

```js
// non-strict
function A(x) {
  this.x = x;
}

var a = A(10); // forget "new" keyword

// as a result "a" is undefined,
// because exactly this value is returned
// implicitly from the A function

alert(a); // undefined

// and again created "x" property
// of the global object, because "this"
// is coerced to global object in the
// non-strict in such case

alert(x); // 10

```

In strict mode, an exception is thrown:

```js
"use strict";

function A(x) {
  this.x = x;
}

// forget "new" keyword,
// error, because undefined.x = 10

var a = A(10);

var b = new A(10); // OK
```
