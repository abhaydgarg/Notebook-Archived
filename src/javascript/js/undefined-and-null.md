# Undefined and null

## Undefined

In JavaScript there is `Undefined (type)` and `undefined (value)`.

### Undefined (type)

> is a built-in JavaScript type.

#### Usage

To check if variable really exist without generating "ReferenceError". If variable does not exist in any scope including global and you try to use it then system generate ReferenceError.

```js
//x not declared in any scope

if(x) { // generate ReferenceError, cannot use x if not available in the system.

}

//But to check if available in the system without ReferenceError then use 'typeof'

if(typeof x === 'undefined') {
  console.log('x does not exist.');
}
```

### undefined (value)

> Any variable whose value is undefined has `typeof` equal to undefined.

#### Usage

##### [1]. Any variable that has not been assigned a value, JavaScript automatically provide the `undefined` value in the background

```js
// x is declared but not initialized yet (no value is given)

var x;

// without the use of typeof, here we are checking the value not type.
// @see undefined is not enclosed within quotes. With typeof check it is enclosed.

if(x === undefined) {
  console.log('x exists but no value given.');
}
```

##### [2]. A function without a `return` statement, or a function with an empty `return` statement returns `undefined`

```js
var c = function() {

};
console.log(c()); // print 'undefined'

var d = function() {
  return;
};
console.log(d()); // print 'undefined'
```

##### [3]. The value of an un-supplied function argument is `undefined`

```js
function fx (x) {
  return x;
}

//fx() called without supplying argument x, so JavaScript automatically assign undefined value.

console.log(fx()); //print 'undefined'
```

##### [4]. Object keys

If you try to access the key which is not present in object then it has `undefined` value.

```js
var obj = {'x': 1};

console.log(obj.y) // undefined

//check
if(obj.y === undefined) {

}
```

## null

> Note: lowercase not uppercase.

It has value `null` , there is a value but it is a way to say through `null` value that _**Absence of value**_.

```js
//foo is known to exists but it has no value:
var foo = null;
console.log(foo) // print 'null'
```

> Note: `typeof null;` is `object` not `null`.

## Difference between `null` and `undefined`

`null` means variable exists but has no value whereas `undefined` means variable exists but has value `undefined`.

```js
typeof null        // object (bug in ECMAScript, should be null)
typeof undefined   // undefined
null === undefined // false
null == undefined // true
```
