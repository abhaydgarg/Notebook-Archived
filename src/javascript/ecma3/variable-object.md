# Variable object

Many ECMAScript programmers know that variables are closely related with the **execution context.**

```js
var a = 10; // variable of the global context

(function () {
  var b = 20; // local variable of the function context
})();

alert(a); // 10
alert(b); // "b" is not defined
```

There are only two kind of scope, one is called global and another is local. Only function creates local scope and everything other than function fall into global scope. The block of `for` loop in ECMAScript does not create a local context.

```js
for (var k in {a: 1, b: 2}) {
  alert(k);
}

alert(k); // variable "k" still in scope even the loop is finished
```

Let’s see in more details what occurs when we declare our data.

## Data declaration

A _variable object (in abbreviated form — VO)_ is a special object related with an execution context and which stores:

* variables (with `var` declaration).
* function declarations (FunctionDeclaration, in abbreviated form FD), not function expression. Function expression are not included in to VO.
* and function formal parameters.

> Notice, in ES5 the concept of _variable object_ is replaced with _lexical environments_ model.

Schematically, it is possible to present variable object as a normal ECMAScript object:

```js
VO = {};
```

And as we said, VO is a property of an execution context:

```js
activeExecutionContext = {
  VO: {
    // context data (var, FD, function arguments)
  }
};
```

For example:

```js
var a = 10;

function test(x) {
  var b = 20;
};

test(30);
```

And corresponding variable objects are:

```js
// Variable object of the global context
VO(globalContext) = {
  a: 10,
  test: <reference to function>
};

// Variable object of the "test" function local context
VO(test functionContext) = {
  x: 30,
  b: 20
};
```

## Variable object in global context

_Global object_ is the object which is created before entering any execution context; this object exists in the single copy, its properties are accessible from any place of the program, the life cycle of the global object ends with program end.

The global object is initialized with properties such as `Math`, `String`, `Date`, `parseInt` etc., and also in browser environment, `window` property of the global object refers back to global object (however, not in all implementations):

```js
global = {
  Math: <...>,
  String: <...>
  ...
  ...
  window: global // refer back to global object
};
```

So, coming back to variable object of the global context — variable object is the _global object itself_.

So variable object is the _global object itself_ that’s why we have an ability to refer global variables via property names of the global object without prefix.

```js
VO(globalContext) === global;
```

```js
String(10); // means global.String(10);

// with prefixes
window.a = 10; //
global.window.a // 10
global.a // 10;
this.b = 20; // global.b = 20;
```

## Variable object in function context

Execution context of function or VO of function is inaccessible directly, and its role is played by an _activation object (in abbreviated form — AO)_.

```js
// VO of function context is called AO
VO(functionContext) === AO;
```

An _activation object_ is created on entering the context of a function and initialized by property `arguments` which is actually a _Arguments object_:

```js
AO = {
  arguments: <ArgO>
};
```

_Arguments_ object is a property of the activation object. It contains the following properties:

* _callee_ — the reference to the current function.
* _length_ — quantity of actual_ passed_ arguments.
* _properties-indexes_ -  Quantity of these properties-indexes is equal to `arguments.length`.

```js
function foo(x, y, z) {

  // quantity of defined function arguments (x, y, z)
  alert(foo.length); // 3

  // quantity of actual passed arguments (only x, y)
  alert(arguments.length); // 2

  // reference of a function to itself
  alert(arguments.callee === foo); // true

  // parameters sharing
  alert(x === arguments[0]); // true
  alert(x); // 10

  arguments[0] = 20;
  alert(x); // 20

  x = 30;
  alert(arguments[0]); // 30

  // however, for not passed argument z,
  // related index-property of the arguments
  // object is not shared

  z = 40;
  alert(arguments[2]); // undefined

  arguments[2] = 50;
  alert(z); // 40

}

foo(10, 20);
```

> In ES5 the concept of activation object is also replaced with common and single model of lexical environments.

## Phases of processing the context code

Now we have reached at the main point. Processing of the execution context code is divided on two basic stages:

1. Entering the execution context **(Compile Code)**.
2. Code execution **(Run Code)**.

### Entering the execution context

#### Compile Code

On entering the execution context (but _before_ the code execution), VO is filled with the following properties in the order mentioned below.** **

##### [1]. Formal parameters first

Interpreter creates VO object property and value from formal parameters of function. If parameter is not passed or missing then still property is created in VO object but with `undefined` value.

##### [2]. then function declaration

Interpreter creates VO object property with name of function declaration. If VO object already contains a property then it value of existing property with new one.

##### [3]. at the end variable declaration

Interpreter creates VO object property with name of variable and value `undefined`. If VO object already contains a property then the variable declaration does not disturb the existing property.

```js
function test(a, b) {
  var c = 10;
  function d() {}
  var e = function _e() {};
  (function x() {});
}

test(10); // call
```

On _entering_ the `test` function context with the passed parameter `10`, AO is the following:

```js
AO(test) = {
  a: 10,
  b: undefined,
  c: undefined,
  d: <reference to FunctionDeclaration "d">
  e: undefined
};
```

Notice, that AO does not contain function `x`. This is because `x` is not a function declaration but the _function-expression (FunctionExpression, in abbreviated form FE)_ which _does not affect VO_.

However, function `_e` is also a function-expression, but because of assigning it to the variable `e`, it becomes accessible via the `e` name.

### Code execution

#### Run code

By this moment, AO/VO is already filled by properties (though, not all of them have the real values and most of them yet still have initial value `undefined`).

Consider privious example; at the time of code execution phase the AO/VO are modified as follows:

```js
AO(test) = {
  a: 10,
  b: undefined,
  c: 10,
  d: <reference to FunctionDeclaration "d">
  e: <reference to FunctionExpression "_e">
};
```

But the function expression `x` is not in the AO/VO. If we try to call `x` function _before_ or _even after_ definition, we get an error: `"x" is not defined`.

#### How value can be changed at the execution phase

```js
alert(x); // function

var x = 10;
alert(x); // 10

x = 20;

function x() {}

alert(x); // 20
```

Why in the first alert `x` is a function and also accessible _before_ the declaration? Why it’s not `10` or `20`? Because, according to the rule — VO is filled with function declarations before variable declaration _on entering the context, \_and variable declaration with same name \_do not disturb_ the value of the _already declared function or formal parameter with the same name_. Therefore, on entering the context VO is filled as follows:

```js
// Compile phase
VO = {};

VO['x'] = <reference to FunctionDeclaration "x">

// found var x = 10;
// if function "x" would not be already defined
// then "x" will be undefined, but in this case
// variable declaration does not disturb
// the value of the function with the same name

VO['x'] = <the value is not disturbed, still function>
```

And then at code execution phase, VO is modified as follows:

```js
// execution phase
VO['x'] = 10;
VO['x'] = 20;
```

##### Another example

In the example below we see again that variables are put into the VO on entering the context phase (so, the `else` block is _never executed_, but nevertheless, the variable `b` _exists_ in VO):

```js
if (true) {
  var a = 1;
} else {
  var b = 2;
}

alert(a); // 1
alert(b); // undefined value
```
