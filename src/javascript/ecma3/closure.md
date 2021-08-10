# Closure

As is known, in functional languages (and ECMAScript supports this paradigm and stylistics), functions are _data_, i.e. they can be _assigned to variables_, passed as _arguments_ to other functions, _returned_ from functions etc. Such functions have special names and structure.

A _functional argument (“Funarg”)_ — is an argument whose value is a function.

```js
function exampleFunc(funArg) {
  funArg();
}

exampleFunc(function () {
  console.log('funArg');
});
```

The actual parameter related with the “funarg” in this case is the anonymous function _passed_ to the `exampleFunc` function. In turn, the function which _receives_ another function as the argument is called a _**higher-order function (HOF)**_.

Functions which can participate as normal data, i.e. be passed as arguments, receive functional arguments or be returned as functional values, are called _**first-class functions**_. In ECMAScript _all_ functions are _first-class_.

## Auto-applicative or Self-applicative

A function which receives itself as an argument.

```js
(function selfApplicative(funArg) {

  if (funArg && funArg === selfApplicative) {
    console.log('self-applicative');
    return;
  }

  selfApplicative(selfApplicative);

})();
```

## Auto-replicative or Self-replicative

A function which returns itself.

```js
(function selfReplicative() {
  return selfReplicative;
})();
```

## Funarg problem

In stack-oriented programming languages local variables of functions are stored on a stack which is pushed with these variables and function arguments every time when the function is called.

On return from the function the variables are removed from the stack. This model is a big restriction for using functions as functional values (i.e. returning them from parent functions). Mostly this problem appears when a function uses free variables.

A _**free variable**_ is a variable which is used by a function, but is neither a parameter, nor a local variable of the function.

```js
function testFn() {

  var localVar = 10;

  function innerFn(innerParam) {
    console.log(innerParam + localVar);
  }

  return innerFn;
}

var someFn = testFn();
someFn(20); // 30
```

In this example `localVar` variable is _free_ for the `innerFn` function.

If this system had use a _stack-oriented_ model for storing local variables, it would mean that on return from `testFn` function all its local variables would be _removed\_from the stack. And this would cause an error at _`innerFn`_ function activation from the \_outside_. Moreover, in this particular case, in the stack-oriented implementation, returning of the `innerFn` function would not be possible at all, since `innerFn` is also _local_ for `testFn` and therefore is also removed on returning from the `testFn`.

**Another problem** of functional objects is related with passing a function as an argument in a system with dynamic scope implementation.

```js
var z = 10;

function foo() {
  console.log(z);
}

foo(); // 10 – with using both static and dynamic scope

(function () {

  var z = 20;
  foo(); // 10 – with static scope, 20 – with dynamic scope

})();

// the same with passing foo
// as an arguments

(function (funArg) {

  var z = 30;
  funArg(); // 10 – with static scope, 30 – with dynamic scope

})(foo);
```

We see that in systems with dynamic scope, _variable resolution_ is managed with a _dynamic (active) stack of variables_. Thus, free variables are searched in the _dynamic chain_ of the _current activation_ — in the place where the function is _called_, but not in the _static (lexical) scope chain_ which is saved at _function creation_.

And this can lead to ambiguity. Thus, even if `z` exists (in contrast with the previous example where local variables would be removed from a stack), there is a question: which value of `z` (i.e. `z` from which _context_, from which _scope_) should be used in various calls of `foo` function?

The described cases are two types of the _funarg problem_ — depending on whether we deal with the _functional value_ returned from a function _(upward funarg)_, or with the _functional argument_ passed to the function _(downward funarg)_.

**For solving this problem (and its subtypes) the concept of a **_**closure**_** was proposed.**

## Closures

```js
var x = 20;

function foo() {
  console.log(x); // free variable "x" == 20
}

// Closure for foo
fooClosure = {
  call: foo // reference to function
  lexicalEnvironment: {x: 20} // context for searching free variables
};
```

In the example above, `fooClosure` of course is a pseudo-code whereas in ECMAScript `foo` function already contains as one of its internal property a scope chain of a context in which it has been created.

**Closures solution to "Funarg problem".**

Closure saves its parent variables in the _lexical place_ of the _source code_, that is — where the function is _defined_. At next activations of the function, free variables are searched in this saved _(closured)_ context, that we can see in examples above where variable `z` always should be resolved as `10` in ECMAScript.

## ECMAScript closures implementation

Here it is necessary to notice that ECMAScript uses only static (lexical) scope (whereas in some languages, for example in Perl, variables can be declared using both static or dynamic scope).

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
```

Technically, the variables of a parent context are saved in the internal `[[Scope]]` property of the function. _All functions in ECMAScript are closures_, since _all_ of them at creation save scope chain of a parent context. The important moment here is that, regardless — whether a function will be activated later or not — the parent scope _is already saved_ to it _at creation moment_:

```js
var x = 10;

function foo() {
  console.log(x);
}

// foo is a closure
foo: <FunctionObject> = {
  [[Call]]: <code block of foo>,
    global: {
      x: 10
    }
  ],
  ... // other properties
};
```

### One `[[Scope]]` value for “them all”

`[[Scope]]` or parent context is shared among all inner functions. So if any change to parent's variable in any inner function reflect the change in other inner function too.

It is necessary to notice that closured `[[Scope]]` in ECMAScript is the _same_ object for the several inner functions created in this parent context. It means that modifying the closured variable from one closure, _reflects_ on reading this variable in _another_ closure.

**All **_**inner**_** functions **_**share**_** the **_**same parent**_** scope.**

```js
var firstClosure;
var secondClosure;

function foo() {

  var x = 1;

  firstClosure = function () { return ++x; };
  secondClosure = function () { return --x; };

  x = 2; // affection on AO["x"], which is in [[Scope]] of both closures

  console.log(firstClosure()); // 3, via firstClosure.[[Scope]]
}

foo();

console.log(firstClosure()); // 4
console.log(secondClosure()); // 3
```

**Another example:** to illustrate the sharing mechanism of parent's scope.

There is a widespread mistake related with this feature. Often programmers get unexpected result, when create functions in a _loop_, trying to associate with every function the loop’s counter variable, expecting that every function will keep its “own” needed value.

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

Accordingly, at the moment of function activations or called, last assigned value of `k` variable, i.e. `3` is used.

Creation of additional enclosing context may help to solve the issue:

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
```

First, the function `_helper` is created and _immediately activated_ with the argument `k`.

Then, returned value of the `_helper` function is _also a function_, and exactly _it_ is saved to the corresponding element of the `data` array.

This technique provides the following effect: being activated, the `_helper` every time creates a _new activation object_ which has argument `x`, and the _value_ of this argument is the _passed_ value of `k` variable.

Thus, the `[[Scope]]` of returned functions is the following:

```js
data[0].[[Scope]] === [
  ... // higher variable objects
  AO of the parent context: {data: [...], k: 3},
  AO of the _helper context: {x: 0}
];

data[1].[[Scope]] === [
  ... // higher variable objects
  AO of the parent context: {data: [...], k: 3},
  AO of the _helper context: {x: 1}
];

data[2].[[Scope]] === [
  ... // higher variable objects
  AO of the parent context: {data: [...], k: 3},
  AO of the _helper context: {x: 2}
];
```

**In ES6, the above problem can be solved very easily using **`let`**, or **`const`**  variable declarations.**

```js
let data = [];

for (let k = 0; k < 3; k++) {
  data[k] = function () {
    console.log(k);
  };
}

// Also correct output.
data[0](); // 0
data[1](); // 1
data[2](); // 2
```
