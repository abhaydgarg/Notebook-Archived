# Spread and Rest operators

## Rest

It is used for **function arguments** only when **function is define** not when function is called and used to define indefinite numbers of arguments as an **array**.

```js
// ### function is define and all the arguments are packed in array theArgs
function fun(...theArgs) {
  console.log(theArgs.length);
}

fun() // 0 length
fun(1) // 1 length and 1 argument packed into array [1]
fun(1, 2, 3) // 3 length and all arguments are packed into array [1, 2, 3]

// ### Other way to use it
function fun(param, ...theArgs) {
}

fun(1, 2, 3) // param === 1 and theArgs === [2, 3]
```

## Spread

This can be used for various purposes.

### Function call

```js
// ### When you want to provide arguments packed in an array
fun(x, y, z) {
  console.log(x + y + z);
}

// we have array say
var arr = [1, 2, 3];

// arr can be passed during function call
fun(...arr); // 6
// same as
fun(1, 2, 3) // 6
```

### In Array

```js
// ### 1. Copy array
var arr = [1, 2, 3];
var arr2 = [...arr]; // Before arr.slice() to copy an array
arr2.push(4);

console.log(arr) // [1, 2, 3]
console.log(arr2) // [1, 2, 3, 4]

// ### 2. Concat or join arrays
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr1, ...arr2]; // Before arr1 = arr1.concat(arr2);

// OR

var arr = [1, 2];
arr = [...arr, 3, 4];
console.log(arr); // [1, 2, 3, 4]
```

### In Object

Before: `Object.assign()` to copy and merge objects.

> It never changes the objects that are going to be merged. It always returned new object reference.

```js
// ### 1. Clone object - Does SHALLOW-CLONING means excluding prototype
var obj1 = { foo: 'bar', x: 42 };
var clonedObj = { ...obj1 };

// change obj1
obj1.y = 1;

console.log(obj1) // { foo: 'bar', x: 42, y: 1 }
console.log(clonedObj) // unchanged { foo: 'bar', x: 42 }

// ### 2. Merge objects
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj) // { foo: "baz", x: 42, y: 13 }
// Notice: same key from obj2 replace the value of obj1

// OR

var obj1 = { foo: 'bar', x: 42 };
var mergedObj = { ...obj1, foo: 'baz', z: 14 };
console.log(mergedObj) // { foo: "baz", x: 42, y: 13, z: 14}
```
