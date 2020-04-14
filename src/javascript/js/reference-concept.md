# Reference concept

**Object and Array are reference data types** and all other primitive types are value based data types. See example below:

```js
//### Primitive types
var x = 1; //integer
var y = x;

x = 2;

console.log(x);//2
console.log(y); //1
//the value of y is unchanged because the copy of x value is assigned to y.

//### In case of object
var obj = {
   x: 1
};
var obj1 = obj;
obj.x = 2;

//any change to obj property x reflect to x property of obj1.
console.log(obj.x);   //2
console.log(obj1.x); //2

//### In case of Array
var a = [1, 2];

var b = a;

// add new element in b
b.push(3);

// reflect change in a too
console.log(a); // [1, 2, 3]
console.log(b); // [1, 2, 3]
```

**Situation where object reference mechanism can fool you.**

Check out the example below:

```js
function test() {

  var obj = {};
  var collect = [];

  for (var i = 0; i <= 1; i++) {
    obj.x = i + 1;
    obj.y = i + 2;
    collect.push(obj);
  }
  console.log(collect);
}

test();

//[{x:2, y:3}, {x:2, y:3}]
```

In above example, you supposed to get different key values `[{x:1, y:2}, {x:2, y:3}]`. But you get both objects with same key values. Because, when control runs the loop for first time the key values are set to `{x:1, y:2}` and push to the array. Second time when loop runs, the value of `obj.x` changed to 2. Now, the object that we have pushed to array before still refer/point to `obj`. So the value of `obj.x` is also changed in object pushed to an array previously. No matter how many times loop runs and how many objects are pushed to array called `collect` the value of keys would always be same for all objects in `collect` and which will be equal to the last object that is pushed.

**To avoid the problem,** you must declare the object inside loop that means creating the new reference for object every time.

```js
function test() {

  var collect = [];

  for (var i = 0; i <= 1; i++) {
    var obj = {}; // new reference each time with same variable name
    obj.x = i + 1;
    obj.y = i + 2;
    collect.push(obj);
  }
  console.log(collect);
}

test();

//[{x:1, y:2}, {x:2, y:3}]

/***** OR *****/

// just rewrite differently

function test() {

  var obj;
  var collect = [];

  for (var i = 0; i <= 1; i++) {
    obj = {}; // new reference each time with same variable name
    obj.x = i + 1;
    obj.y = i + 2;
    collect.push(obj);
  }
  console.log(collect);
}

test();

//[{x:1, y:2}, {x:2, y:3}]
```

## Not by reference but by share

```js
var objOne = {
  x: 1,
  y: 2
};

var objTwo = objOne;

// change the x vlaue to 2 by objTwo
objTwo.x = 2;

// Change the value of key x in objOne as well
console.log(objOne); // { x: 2, y: 2 }

// Now what if objTwo initialized with empty object
objTwo = {};

console.log(objOne); // { x: 2, y: 2 } but expected output = {}

// As per pass by reference mechanism. objOne is expected to
// log {}, because objTwo was re-initialized with {}.
```

> Most techincal and accurate explanation is called "by share" topic in [Evaluation strategy](/javascript/ecma3/evaluation-strategy)

When you assign one variable to another, it's not that both those variables are now linked by reference; you're misunderstanding what "pass by reference" here.

A variable holding an object does not "directly" hold an object. What it holds is a reference to an object. When you assign that reference from one variable to another, you're making a **copy** of that reference. Now both variables hold a copy of reference to an object. Modifying the object through that reference changes it for both variables holding a reference to that object.

When you assign a new value to one of the variables, you're just modifying the value that variable holds. The variable now ceases to hold a reference to the object, and instead holds something else. The other variable still holds its reference to the original object, the assignment didn't influence it at all.

**Lets understand pass by reference step by step:**

```js
/** Interpreter does:
 * 1. Create object in memory with value {x:1, y:2} and create memory
 *    reference (pointer) say 'ref-111', which points to the object.
 *
 * 2. The reference value is then assign to variable 'objOne'. Hence,
 *    'objOne = ref-111'. 'objOne' does not hold actual value
 *    {x:1, y:2} but reference value (ref-111).
 */

var objOne = {
  x: 1,
  y: 2
};

/**
 * Interpreter does:
 * Create variable 'objTwo' and assign/copy value of
 * 'objOne', which is a reference value 'ref-111'.
 * Now, 'objOne = ref-111' and 'objTwo = ref-111'.
 */
var objTwo = objOne;

/**
 * Interpreter does:
 * 1. Create object in memory with value {} and create memory
 *    reference (pointer) value say 'ref-222' which point to the object.
 *
 * 2. The reference value is then assign/copy to variable 'objTwo'.
 *    Hence, 'objTwo = ref-222'.
 *    Now, 'objOne = ref-111' and 'objTwo = ref-222'.
 */
objTwo = {};

//Therefore, 'console.log(objOne)' is not expected to log {}.
```

## Shallow copy

> Shallow copy means copy only the reference.

**Arrays and Object in Javascript performs the Shallow copy by default.**

And change in copied object reflects in original object and vice versa.

```js
//### In case of object
var obj = {
   x: 1
};
var obj1 = obj;
obj.x = 2;

//any change to obj property x reflect to x property of obj1.
console.log(obj.x);   //2
console.log(obj1.x); //2

//### In case of Array
var a = [1, 2];

var b = a;

// add new element in b
b.push(3);

// reflect change in a too
console.log(a); // [1, 2, 3]
console.log(b); // [1, 2, 3]
```

## Deep copy

Deep copy means copy the object properties recursively into the new object. In deep copy, Change in copied object does not reflect in original object and vice versa. All primitive data types in javascript performs deep copy by default. Boolean, Null, Undefined, Number, String etc… are Primitive data types in javascript.

> **There is no 100% pure native mechanism to perform a deep copy for object in JavaScript** and one of the reason is that it’s quite complicated. For instance, there could be circular references (Circular objects) that are usually hard to deal with and there could be deep nested objects and doing deep copying could be a quite slow operation and memory consumption process.

### Circular object

Circular objects are objects that have properties referencing themselves. Let's use the methods of copying objects we've learnt so far to make copies of a circular object and see if it works.

```js
let obj = {
  a: 'a',
  b: {
    c: 'c',
    d: 'd',
  },
}

obj.c = obj.b;
obj.e = obj.a;
obj.b.c = obj.c;
obj.b.d = obj.b;
obj.b.e = obj.b.c;

//### Using JSON.parse(JSON.stringify())
// TypeError: Converting circular structure to JSON
let newObj = JSON.parse(JSON.stringify(obj));
console.log(newObj);

//### Using Object.assign()
// works fine for shallow copying circular objects
// but wouldn't work for deep copying.
let newObj2 = Object.assign({}, obj);
console.log(newObj2);
```

## Options available in Javascript for copying

### Object.assign()

It performs **shallow copy** and it merges own enumerable keys, giving priority right to left.

#### [1]. Nested object scenario

```js
//### When we have object with only one level, means no nested object scenario
// in that case we can achieve deep copy
let obj = {
  a: 1,
  b: 2,
};
let objCopy = Object.assign({}, obj);
console.log(objCopy); // { a: 1, b: 2 }
// update property b
objCopy.b = 89;
console.log(objCopy); // { a: 1, b: 89 }
console.log(obj); // { a: 1, b: 2 }

//### What happen when we have nested object scenario?
let obj = {
  a: 1,
  b: {
    c: 2,
  },
}
let newObj = Object.assign({}, obj);
console.log(newObj); // { a: 1, b: { c: 2} }

// no change reflected
newObj.a = 20;
console.log(obj); // { a: 10, b: { c: 2} }
console.log(newObj); // { a: 20, b: { c: 2} }

// nested object - copied by reference
// change in newObj reflect change in obj
newObj.b.c = 30;
console.log(obj); // { a: 10, b: { c: 30} }
console.log(newObj); // { a: 20, b: { c: 30} }
```

#### [2]. a getter and a setter, will be copied as data

```js
var special = {
  get next() {
    return ++this.counter;
  },
  counter: 0
};

// what could possibly go wrong?
var notSpecial = Object.assign({}, special);

notSpecial.counter; // 1 if `next` was copied before `counter`
                    // 0 if keys order is different: WATCH OUT!
```

> Don’t use `Object.assign` for anything but simple shallow copies or merges with data you know for sure does NOT contain accessors.

#### [3]. Properties on the prototype chain and non-enumerable properties cannot be copied

```js
let someObj = {
  a: 2,
}

let obj = Object.create(someObj, {
  b: {
    value: 2,
  },
  c: {
    value: 3,
    enumerable: true,
  },
});

let objCopy = Object.assign({}, obj);
console.log(objCopy); // { c: 3 }

// 1. someObj is on obj's prototype chain so it wouldn't be copied.
// 2. property b is a non-enumerable property.
// 3. property c has an enumerable property descriptor allowing it to be enumerable.
// That's why it was copied.
```

### JSON.parse(JSON.stringify())

#### [1]. Nested object scenario

```js
let obj = {
  a: 1,
  b: {
    c: 2,
  },
}

let newObj = JSON.parse(JSON.stringify(obj));

obj.b.c = 20;
console.log(obj); // { a: 1, b: { c: 20 } }
console.log(newObj); // { a: 1, b: { c: 2 } } (New Object Intact!)
```

> Unfortunately, this method can't be used if you want to to copy object's methods too. See example below:

```js
let obj = {
  name: 'scotch.io',
  exec: function exec() {
    return true;
  },
}

//### using Object.assign() - Copy method too
let method1 = Object.assign({}, obj);
console.log(method1);
/* result
{
  exec: function exec() {
    return true;
  },
  name: "scotch.io"
}
*/

//### using JSON.parse(JSON.stringify()) - Exclude methods
let method2 = JSON.parse(JSON.stringify(obj));
console.log(method2);
/* result
{
  name: "scotch.io"
}
*/
```

### Spread token `...`

> This also support shallow copy only.
