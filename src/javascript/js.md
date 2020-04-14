# Javascript

## Declarations with `var`  always

When you fail to specify `var`, the variable gets placed in the global context, potentially clobbering existing values. Also, if there's no declaration, it's hard to tell in what scope a variable lives.

## JavaScript is case-sensitive

```js
// foo and Foo are two different variables.
var foo;

var Foo;

//### In case of function

// foo and FOO are two different functions

function foo() {}
function FOO() {}

//### Gotcha

function foo() {
 console.log(1);
}
function foo() {
  console.log(2);
}

// in this case where we have two functions with the same name, then last function override the
// first one. If you check how Variable object is formed, it will become clear.

foo() // 2 not 1
```

## Clarification: semicolons and functions

Semicolons should be included at the end of function expressions, but not at the end of function declarations. The distinction is best illustrated with an example:

```js
var foo = function() {
  return true;
};  // semicolon here.

function foo() {
  return true;
}  // no semicolon here.
```

## Function declarations within block

Do not do this:

```js
if (x) {
  function foo() {}
}
```

Instead use a variable, initialized with a Function Expression to define a function within a block:

```js
if (x) {
  var foo = function() {};
}
```

## Wrapper objects for primitive types

Never use wrapper objects for generating and type casting primitive types.

```js
var x = new Boolean(false); // wrapper is used

// you think 'x' would be false but no it is true because type of 'x' is object
// due wrapper object. Therefore, control enters the condition.
if (x) {
  alert('hi');
}

typeof Boolean(0) == 'boolean'; // new not used
typeof new Boolean(0) == 'object'; //new used
```

1. Use `{}` instead of `new Object()`
2. Use `""` instead of `new String()`
3. Use `0` instead of `new Number()`
4. Use `false` instead of `new Boolean()`
5. Use `[]` instead of `new Array()`
6. Use `/()/` instead of `new RegExp()`
7. Use `function (){}` instead of `new function()`

## delete

Prefer `this.foo = null`. Instead of delete.

In modern JavaScript engines, changing the number of properties on an object is much slower than reassigning the values. The delete keyword should be avoided except when it is necessary to remove a property from an object.

## Prefer JSON.parse over eval

Do not use:

```js
var userInfo = eval(feed);
var email = userInfo['email'];
```

Use instead:

```js
var userInfo = JSON.parse(feed);
var email = userInfo['email'];
```

With `JSON.parse`, invalid JSON (including all executable JavaScript) will cause an exception to be thrown.

## Multiline string

Do not do this:

```js
var myString = 'A rather long string of English text, an error message \
                actually that just keeps going and going -- an error \
                message to make the Energizer bunny blush (right through \
                those Schwarzenegger shades)! Where was I? Oh yes, \
                you\'ve got an error and all the extraneous whitespace is \
                just gravy.  Have a nice day.';
```

Use string concatenation instead:

```js
var myString = 'A rather long string of English text, an error message ' +
    'actually that just keeps going and going -- an error ' +
    'message to make the Energizer bunny blush (right through ' +
    'those Schwarzenegger shades)! Where was I? Oh yes, ' +
    'you\'ve got an error and all the extraneous whitespace is ' +
    'just gravy.  Have a nice day.';
```

## Use parameter defaults

If a function is called with a missing argument, the value of the missing argument is set to undefined. Undefined values can break your code. It is a good habit to assign default values to arguments.

```js
function myFunction(x, y) {
    if (y === undefined) {
        y = 0;
    }
}
```

## End switch with default

```js
switch (new Date().getDay()) {
    case 0:
        day = "Sunday";
        break;
    case 1:
        day = "Monday";
        break;
    case 2:
        day = "Tuesday";
        break;
    case 3:
        day = "Wednesday";
        break;
    case 4:
        day = "Thursday";
        break;
    case 5:
        day = "Friday";
        break;
    case 6:
        day = "Saturday";
        break;
    default:
        day = "Unknown";
}
```
