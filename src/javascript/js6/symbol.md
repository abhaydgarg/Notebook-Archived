# Symbol

1. New primitive type a unique token that is guaranteed never to clash with another symbol. In this sense, you could think of them as a kind of UUID (Universally Unique Identifier).
2. They're immutable once created. You can't set properties on them (and if you try that in strict mode, you'll get a TypeError).

```js
let foo = Symbol();
let bar = Symbol();

foo === bar // false, because symbol are unique

// ### Symbols can also be created with a label, by passing a string as the first argument.
// The label does not affect the value of the symbol, but is useful for debugging,
// and is shown if the symbol’s toString() method is called.

let foo = Symbol('foo');
console.log(foo); // Symbol(baz)
```

## When to use

### Class/Module constant

```js
class Application {
  constructor(mode) {
    switch (mode) {
      case Application.DEV:
        // Set up app for development environment
        break;
      case Application.PROD:
        // Set up app for production environment
        break;
      default:
        throw new Error('Invalid application mode: ' + mode);
    }
  }
}

Application.DEV = Symbol('dev');
Application.PROD = Symbol('prod');

// Example use
let app = new Application(Application.DEV);

// String and integers are not unique values; values such as the
// number 2 or the string ‘development’,
// for example, could also be in use elsewhere in the program for different purposes.
// Using symbols means we can be more confident about the value being supplied.
```

### Property key of object

```js
const MY_KEY = Symbol();
let obj = {
};
```

#### Advantages

1. You can be sure that symbol-based keys will never clash, unlike string keys, which might conflict with keys for existing properties or methods of an object if have same name.

2. They won't be enumerated in `for..in` loops, and are ignored by functions such as `Object.keys()`, `Object.getOwnPropertyNames()` and `JSON.stringify()`. This makes them ideal for properties that you don't want to be included when serializing an object.

```js
let user = {};
let email = Symbol();

user.name = 'Fred';
user.age = 30;
user[email] = 'fred@example.com';

Object.keys(user);
// <-- Array [ "name", "age" ]

Object.getOwnPropertyNames(user);
// <-- Array [ "name", "age" ]

JSON.stringify(user);
// <-- "{"name":"Fred","age":30}"
```

## Access symbol keys

```js
let user = {};
let email = Symbol();

user.name = 'Fred';
user.age = 30;
user[email] = 'fred@example.com';

// ### Object.getOwnPropertySymbols() - Returns an array of any symbol-based keys of object.

Object.getOwnPropertySymbols(user); // [ Symbol() ]

// ### Reflect.ownKeys() - Return an array of all keys, including symbols.

[ "name", "age", Symbol() ]
```

## Global registry

**Symbol created in Global Registry are not unique.** Unlike the unique symbols defined by `Symbol()`, symbols in the symbol registry are shared. The registry is useful when multiple web pages, or multiple modules within the same web page, need to share a symbol.

```js
// ### Create

let debbie = Symbol.for('user');
let mike   = Symbol.for('user');

// true, because Symbol are created in global
//registry with same parameter 'user'.
debbie === mike

// ### Access
Symbol.keyFor(debbie); // "user"
```
