# require vs import

## ES6 import

_Modules_ are JavaScript files that are loaded in a different mode (as opposed to _scripts_, which are loaded in the original way JavaScript worked). This different mode is necessary because modules have very different semantics than scripts:

1. Module code automatically runs in strict mode, and there's no way to opt-out of strict mode.
2. Variables created in the top level of a module aren't automatically added to the shared global scope. They exist only within the top-level scope of the module.
3. The value of `this` in the top level of a module is `undefined`.

### Types of export and import

#### [1] Named exports (several per module)

```js
///------ lib.js ------
export const sqrt = Math.sqrt;
export function square(x) {
    return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}

// private function expression
var subtract = function() {
    return 1 - 1;
};

// NOTICE: when exporting `subtract` use curly braces
export {subtract};
// ### OR ###
export {subtract as sub}; // export by another variable, in import `sub` will be available not `subtract`


//------ main.js ------
import { sqrt, square, diag } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5

// ### OR - using as operator to rename assigned variables ###
import { square as sq, diag as dg } from 'lib';
console.log(sq(11)); // 121
console.log(dg(4, 3)); // 5

// ### OR - using * operator ###
import * as lib from 'lib'; // NOTICE: no curly braces
console.log(lib.square(11)); // 121
console.log(lib.diag(4, 3)); // 5
```

#### [2] Default exports (one per module)

```js
//------ myFunc.js ------
export default function () { ... };

//------ main1.js ------
import anyName from 'myFunc'; // NOTICE: can assign value to any valid javascript name and no curly braces are used
anyName();
```

#### [3] Mixed named & default exports

```js
//------ underscore.js ------
export default function (obj) {
    ...
};
export function each(obj, iterator, context) {
    ...
}
export { each as forEach };

//------ main.js ------
// ### 1 way
import _, { each } from 'underscore'; // `_` assign default value and rest can be assigned in next statement after comma

// ### 2 way
import * as _ from 'underscore';
console.log(_); // `_` has default function with the name `default` and can be accessed by:
console.log(_.default()); // accessing default export
```

> Keep in mind, however, that no matter how many times you use a module in `import` statements, the module will only be executed once. After the code to import the module executes, the instantiated module is kept in memory and reused whenever another `import`statement references it.

### Renaming exports and imports

In the first case, suppose you have a function that you'd like to export with a different name. You can use the `as` keyword to specify the name that the function should be known as outside of the module:

```js
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as add };
```

Here, the `sum()` function (`sum` is the _local name_) is exported as `add()` (`add` is the _exported name_). That means when another module wants to import this function, it will have to use the name `add` instead:

```js
import { add } from "./example.js";
```

If the module importing the function wants to use a different name, it can also use `as`:

```js
import { add as sum } from "./example.js";
console.log(typeof add);            // "undefined"
console.log(sum(1, 2));             // 3
```

This code imports the `add()` function using the _import name_ and renames it to `sum()` (the local name). That means there is no identifier named `add` in this module.

## CommonJS require

You can either use `module.exports` or `exports`. And it is just an object.

> Recommended to use `module.exports`

### Only one item to export in file

```js
///--- file1
module.exports = 'Hello message';

///--- file2
const msg = require('./file1'); // can be any identifier, in this case `msg`
console.log(msg); // Hello message

/**
* Note: If you try to assign value again to `module.exports` then previous one will be override.
*/

///-- file1
module.exports = 'Hello message';
module.exports = 'Hello message1';

///--- file2
const msg = require('./file1');
console.log(msg); // Hello message1 {not Hello message}

/**
* You can assign any type to `module.exports` and it becomes that type
*/

// 1. String

///--- file1
module.exports = 'I am string';

///-- file2
const msg = require('./file1');
console.log(msg); // I am string
console.log(typeof msg); // `string` - Note: string type

// 2. Function

///--- file1
module.exports = function() {
  return 'I am function';
};

///-- file2
const fn = require('./file1');

console.log(fn()); // I am function
console.log(typeof fn); // 'function' - Note: function type

// 3. Object

///--- file1
module.exports = {
  x: 1,
  y: 2
}

///-- file2
const obj = require('./file1');

console.log(obj); // { x: 1, y: 2 }
console.log(typeof obj); // 'object' - Note: object type
```

### More than one items to export in file

Use `module.exports` as object and items to export as properties of object.

```js
///-- file1
module.exports.str = 'I am string';

module.exports.num = 1;

module.exports.fn = function() {
  return 'I am function';
}

module.exports.obj = {
  x: 1,
  y: 2
};

///-- file2
const obj = require('./file1');

console.log(obj);
/*
{
  str: 'I am string',
  num: 1,
  fn: [Function],
  obj: { x: 1, y: 2 }
}
*/
console.log(typeof obj); // `object`
console.log(obj.str) // I am string
console.log(obj.num) // 1
```

_**More ways for multiple export**_

```js
//# Way 1

///-- file1
let str = 'I am string';
let num = 1;

module.exports.str = str;
module.exports.num = num;

///--file2
const obj = require('./file1');

console.log(obj.str); // I am string
console.log(num); // 1

//# Way 2

///-- file1
let str = 'I am string';
let num = 1;

module.exports = {
  str,
  num
}

///--file2
const obj = require('./file1');

console.log(obj.str); // I am string
console.log(num); // 1
```

## `require` is dynamic whereas `import` is static

```js
//# import - Static

/*
  1. you cannot import anywhere in the code dynamicaly.
  If you want to import then you have to to do it at
  the top of the file.
*/

function tryImport() {
    import flag from "./example";    // error
}
// OR
if (flag) {
   import flag from "./example";    // error
}

//correct way
import flag from "./example";

if (flag) {
 //..code
}

/*
  2. you cannot dynamically set the import file name or path.
*/

var path = '/lib';
import flag from `${path}/example`; // error

//OR

var filename = 'example';
import flag from `./${filename}`; // error
```

```js
//# require - Dynamic

if (flag) {
   let flag = require("./example");    // no error - can require dynamically
}

var filename = 'example';
let flag = require(`./${filename}`); // no error - can set file name or path dynamically
```
