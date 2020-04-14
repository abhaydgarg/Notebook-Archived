# Block binding

## let

Inside of a block (indicated by the `{` and `}` characters).

### [1]

Since `let` declarations are not hoisted to the top of the enclosing block, you may want to always place `let` declarations first in the block, so that they are available to the entire block.

```js
function getValue(condition) {
    console.log(a); // reference error that a is not defined, no hoisting
    let a = 1;
    if (condition) {
        console.log(a); // 1
    } else {
        console.log(a); // 1
    }
    console.log(a); // 1
}
```

### [2]

Inside if and loops, let variable become local and cannot access outside `{ }`.

```js
function getValue() {
  let b = 2;
  if(true) {
    console.log(b); // 2 because function local variable can be accessed inside if statement
    let a = 1;
  }
  console.log(a); // reference error because a can only be accessed inside if statement.
```

### [3]

As far as scope chain lookup is concern `let` behaves like `var` that means if let variable is not found in function local scope then js look the variable in parent function scope if there is one or global scope. If not found in scope chain then `undefined` value is assigned.

```js
let a = 1; //global scope

function outer() {
  let a = 2;
  return function () {
    console.log(a);
  }
}

outer()();
// 2, picks parent's a. If you comment the line number 4 then
// a will be 1 that is picked from global scope.
```

### No redecration

If variable is already declared by `var` in current scope then you cannot redeclare it with `let` again in same scope.

```js
var count = 30;
// Syntax error
let count = 40;

// ########## AND ############
// no syntax error because count is redeclare with let in own block
var count = 30;
// Does not throw an error
if (condition) {
    let count = 40;
}
```

## const

Variables declared using `const` are considered _constants_, meaning their values cannot be changed once set.

1. Its scope is same as `let` and the declaration is never hoisted.
2. Cannot redeclare variable with `var` or `let` again in same scope. Same as in case of `let` declaration.
3. Must be initialized with value when declared. Means value must be given for first time otherwise it generate error.

```js
// Valid constant
const maxItems = 30;

// Syntax error: missing initialization
const name;
```

### const vs object

When using `const` to declare object then you:

1. Cannot change or bind another value to object. The default behavior of `const` declaration.
2. Can change the value of object's property.

```js
const person = {
    name: "Nicholas"
};

// works
person.name = "Greg";

// throws an error
person = {
    name: "Greg"
};
```

## Temporal dead zone

A variable declared with either `let` or `const` cannot be accessed until after the declaration. Attempting to do so results in a reference error, even when using normally safe operations such as the `typeof` operation in this example:

```js
if (condition) {
    console.log(typeof value);  // ReferenceError! even if using safe `typeof` method.
    let value = "blue";
}
```

The term **TDZ** is often used to describe why `let` and `const` declarations are not accessible before their declaration.

When a JavaScript engine looks through an upcoming block and finds a variable declaration, it either hoists the declaration to the top of the function or global scope (for `var`) or places the declaration in the TDZ (for `let` and `const`). Any attempt to access a variable in the TDZ results in a runtime error. That variable is only removed from the TDZ, and therefore safe to use, once execution flows to the variable declaration.

## Loops & let

Perhaps one area where developers most want block level scoping of variables is within `for` loops, where the throwaway counter variable is meant to be used only inside the loop.

> let creates new binding each time when loop runs. for example below in the case of `i`. Since when the `let` expression is used, every iteration creates a new lexical scope chained up to the previous scope. This has performance implications for using the `let` expression, which is reported [here](https://bugs.chromium.org/p/v8/issues/detail?id=4762).

```js
for (var i = 0; i < 10; i++) {
    process(items[i]);
}

// i is still accessible here
console.log(i);  // 10

// In JavaScript, however, the variable i is still accessible after the loop is
// completed because the var declaration gets hoisted. Using let instead,
// as in the following code, should give the intended behavior.

for (let i = 0; i < 10; i++) {
    process(items[i]);
}
// i is not accessible here - throws an error
console.log(i);

/* for-in loop */
var object = {
      a: true,
      b: true,
      c: true
    };

// doesn't cause an error
for (let key in object) { // creates new binding for `key` each time it encounter in loop
    console.log(key);
}
```

**Behavior of `let` in loop**

For `for`, `for-in` and `for-of` loops the variable declared, for example in above code, variable `i`. The `let` declaration creates a new variable `i` each time through the loop. But that is not true when `i` is declared using `var`.

> It's important to understand that the behavior of `let` declarations in loops is a specially-defined behavior in the specification and is not necessarily related to the non-hoisting characteristics of `let`.

## Loops & const

It is no recommended to use `const` declaration in loops. However, there are different behaviors based on the type of loop you're using.

**`for`loop**

For a normal `for` loop, you can use `const` in the initializer, but the loop will throw a warning if you attempt to change the value.

```js
// throws an error after one iteration
for (const i = 0; i < 10; i++) {
    console.log(i);
}
//An error is thrown when i++ executes because it’s attempting to modify a constant.
```

**`for-in` or `for-of` loop**

A `const` variable behaves the same as a `let` variable. The `for-in` and `for-of` loops work with `const` because the loop initializer creates a new binding for `key` on each iteration through the loop rather than attempting to modify the value of an existing binding.

```js
var object = {
      a: true,
      b: true,
      c: true
    };

// doesn't cause an error
for (const key in object) {
    console.log(key);
}
```

## Global behavior

When `var` is used in the global scope, it creates a new global variable, which is a property on the global object (`window` in browsers). That means you can accidentally overwrite an existing global using `var`.

If you instead use `let` or `const` in the global scope, a new binding is created in the global scope but no property is added to the global object.

```js
// in a browser
let RegExp = "Hello!";
console.log(RegExp);                    // "Hello!"
console.log(window.RegExp === RegExp);  // false

const ncz = "Hi!";
console.log(ncz);                       // "Hi!"
console.log("ncz" in window);           // false
```

> use `const` by default and only use `let` when you know a variable's value needs to change.
