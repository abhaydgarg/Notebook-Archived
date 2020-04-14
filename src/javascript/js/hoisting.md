# Hoisting

In order to know about hoisting, you must know that javascript run code in two stages:

## [1] Creation stage (where hoisting happens)

The order in which entities are scanned are very important at creation stage:

```
1. Create argument object and scan formal parameter of function.

2. Scan function declaration not function expression because function expression are variable assignment
   that has function assigned to it therefore it treated as variable and scan after function declaration.
   If function name already exists it then it overrides.

3. Scan variable declaration (remember the variable with var keyword) and assign undefined value.
   If variable name already exists then bypass it.

4. Detrmine this value.
```

## [2] Execution stage

> This is where assignment of value happens. Also during creation stage if variable name is already exist then interpreter bypass it but in execution stage interpreter overwrite the value.

All variable declarations are hoisted (lifted and declared) to the top of the function, if defined in a function, or the top of the global context, if outside a function.

```js
(function() {

    console.log(typeof foo); // function pointer
    console.log(typeof bar); // undefined
    // reference error because x is not defined with var,
    // so not included at creation stage.
    console.log(x);

    var foo = 'hello',
        bar = function() {
            return 'world';
        };

   x = 1;

    function foo() {
        return 'hello';
    }

}());
```

The questions we can now answer are:

### Why can we access foo before we have declared it?

  If we follow the `creation stage`, we know the variables have already been created before the `activation / code execution stage`. So as the function flow started executing, `foo` had already been defined in the `activation object`.

### Foo is declared twice, why is foo shown to be `function` and not `undefined` or `string`?

  Function Declaration Overrides Variable Declaration When Hoisted.

  Even though `foo` is declared twice, we know from the `creation stage` that functions are created on the `activation object` before variables, and if the property name already exists on the `activation object`, we simply bypass the decleration.

  Therefore, a reference to `function foo()` is first created on the `activation object`, and when the interpreter gets to `var foo`, we already see the property name `foo` exists so the code does nothing and proceeds.

### Why is bar `undefined`?

  `bar` is actually a variable that has a function assignment, and we know the variables are created in the `creation stage` but they are initialized with the value of `undefined`.

### Why is `x` gives an reference error and stop script to execute?

  At the creation stage when interpreter see that `x` does not declare anywhere in the function using keyword `var` then it does not include in the activation object. Therefore, when you try to access it before the statement `x=1` then interpreter does not found it in activation object. Moreover, in case if you remove the statement `console.log(x);` and put it after `x=1` then you will get output 1 because during execution stage when interpreter see the `x=1` then but again not found in the activation object of current function then interpreter add it to the global object as window property in browser and access it using scope chain mechanism.

## Hoisting and execution context

As we know the interpreter runs the code or function or execution context in two steps. One is called **Creation Stage** and other is called **Execution Stage**.

And hoisting is related to the creation stage which is the first step where actvation object is created and how activation object is created determine the hoisting. In above section we already know about it. But before we leave there is one more thing that need to be keep in mind is **how value can be changed at during second stage called execution stage.**

```js
alert(x); // function

var x = 10;
alert(x); // 10

x = 20;

function x() {}

alert(x); // 20
```

> Activation object(AO) in global scope is refer to Variable object(VO).

Why in the first alert `x` is a function and moreover is accessible _before_ the declaration? Why it’s not `10` or `20`? Because, according to the rule — VO is filled with function declarations first _on entering the context_. Also, at the same phase, on entering the context, there is a variable declaration `x`, but as we mentioned above, the step of variable declarations semantically goes _after_ function and formal parameters declarations and on this phase _do not disturb_ the value of the _already declared function or formal parameter with the same name_. Therefore, on entering the context VO is filled as follows:

```js
VO = {};

VO['x'] = <reference to FunctionDeclaration "x">

// found var x = 10;
// if function "x" would not be already defined
// then "x" be undefined, but in our case
// variable declaration does not disturb
// the value of the function with the same name

VO['x'] = <the value is not disturbed, still function>
```

And then at code **execution phase**, VO is modified as follows:

```js
// the x is no longer points to function but get reassign with x variable.
VO['x'] = 10;
VO['x'] = 20;
```

**Another example** - In the example below we see again that variables are put into the VO on entering the context phase so, the `else` block is _never executed_, but nevertheless, the variable `b` _exists_ in VO):

```js
if (true) {
  var a = 1;
} else {
  var b = 2;
}

alert(a); // 1
alert(b); // undefined value
```

## About variables

Often various articles and even books on JavaScript claim that: “it is possible to declare global variables using _var_ keyword (in the global context) and without using `var` keyword in any place such as in function. It is not so. Remember:

_variables are declared only with using var keyword_.

And assignments like:

```js
a = 10;
```

No declaring `a` with `var` before, just create the new _property_ (but _not the variable_) of the global object. “Not the variable” is not in the sense that it cannot be changed, but “not the variable” in concept of variables in ECMAScript.

```js
alert(a); // undefined
alert(b); // "b" is not defined

b = 10;
var a = 20;
```

All again depends on VO and phases of its modifications (_entering the context_ stage and the _code execution_ stage):

```js
VO = {
  a: undefined
};
```

See the `b` is not available at the execution context because it is not declared using keyword `var`. It wil be available at the code execution phase where the assignment value is given to variable.

Let’s change the code:

```js
alert(a); // undefined, we know why

b = 10;
alert(b); // 10, created at code execution

var a = 20;
alert(a); // 20, modified at code execution
```
