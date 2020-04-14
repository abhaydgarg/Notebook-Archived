# Promises

Each promise goes through a short lifecycle starting in the _pending_ state, which indicates that the asynchronous operation hasn't completed yet. A pending promise is considered _unsettled_. The promise in the last example is in the pending state as soon as the `readFile()` function returns it. Once the asynchronous operation completes, the promise is considered _settled_ and enters one of two possible states:

1. _Fulfilled_: The promise's asynchronous operation has completed successfully.
2. _Rejected_: The promise's asynchronous operation didn't complete successfully due to either an error or some other cause.

An internal `[[PromiseState]]` property is set to `"pending"`, `"fulfilled"`, or `"rejected"` to reflect the promise's state. This property isn't exposed on promise objects, so you can't determine which state the promise is in programmatically. But you can take a specific action when a promise changes state by using the `then()` method.

The `then()` method is present on all promises and takes two arguments. The first argument is a function to call when the promise is fulfilled. Any additional data related to the asynchronous operation is passed to this fulfillment function. The second argument is a function to call when the promise is rejected. Similar to the fulfillment function, the rejection function is passed any additional data related to the rejection.

## catch()

Promises also have a `catch()` method that behaves the same as `then()` when only a rejection handler is passed. For example, the following `catch()` and `then()` calls are functionally equivalent:

```js
promise.catch(function(err) {
    // rejection
    console.error(err.message);
});

// is the same as:

promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});
```

## Creating promise

### Unsettle promise - Promise that is added to job scheduling

New promises are created using the `Promise` constructor. This constructor accepts a single argument: a function called the _executor_, which contains the code to initialize the promise. The executor is passed two functions named `resolve()` and `reject()` as arguments. The `resolve()` function is called when the executor has finished successfully to signal that the promise is ready to be resolved, while the `reject()` function indicates that the executor has failed.

```js
// Node.js example

let fs = require("fs");

function readFile(filename) {
    return new Promise(function(resolve, reject) {

        // trigger the asynchronous operation
        fs.readFile(filename, { encoding: "utf8" }, function(err, contents) {

            // check for errors
            if (err) {
                reject(err);
                return;
            }

            // the read succeeded
            resolve(contents);

        });
    });
}

let promise = readFile("example.txt");

// listen for both fulfillment and rejection
promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});
```

Keep in mind that the executor runs immediately when `readFile()` is called. When either `resolve()` or `reject()` is called inside the executor, a job is added to the job queue to resolve the promise. This is called _**job scheduling**_, and if you've ever used the `setTimeout()`or `setInterval()` functions, then you're already familiar with it. In job scheduling, you add a new job to the job queue to say, "Don't execute this right now, but execute it later."

#### Job scheduling

The promise executor executes immediately, before anything that appears after it in the source code. For instance:

```js
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

console.log("Hi!");
```

The output for this code is:

```
Promise
Hi!
```

Calling `resolve()` triggers an asynchronous operation. Functions passed to `then()` and `catch()` are executed asynchronously, as these are also added to the job queue. Here's an example:

```js
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

promise.then(function() {
    console.log("Resolved.");
});

console.log("Hi!");
```

The output for this example is:

```
Promise
Hi!
Resolved
```

Note that even though the call to `then()` appears before the `console.log("Hi!")` line, it doesn't actually execute until later (unlike the executor). That's because fulfillment and rejection handlers are always added to the end of the job queue after the executor has completed.

### Settle promise - Promise that is not added to job scheduling

The `Promise` constructor is the best way to create unsettled promises due to the dynamic nature of what the promise executor does. But if you want a promise to represent just a single known value, then it doesn't make sense to schedule a job that simply passes a value to the `resolve()` function. Instead, there are two methods that create settled promises given a specific value.

#### Promise.resolve()

The `Promise.resolve()` method accepts a single argument and returns a promise in the fulfilled state. That means no job scheduling occurs, and you need to add one or more fulfillment handlers to the promise to retrieve the value. For example:

```js
let promise = Promise.resolve(42);

promise.then(function(value) {
    console.log(value);         // 42
});
```

This code creates a fulfilled promise so the fulfillment handler receives 42 as `value`. If a rejection handler were added to this promise, the rejection handler would never be called because the promise will never be in the rejected state.

#### Promise.reject()

You can also create rejected promises by using the `Promise.reject()` method. This works like `Promise.resolve()` except the created promise is in the rejected state, as follows:

```js
let promise = Promise.reject(42);

promise.catch(function(value) {
    console.log(value);         // 42
});
```

Any additional rejection handlers added to this promise would be called, but not fulfillment handlers.

### Executor errors

If an error is thrown inside an executor, then the promise's rejection handler is called. For example:

```js
let promise = new Promise(function(resolve, reject) {
    throw new Error("Explosion!");
});

promise.catch(function(error) {
    console.log(error.message);     // "Explosion!"
});
```

## Chaining promises

Each call to `then()` or `catch()` actually creates and returns another promise. This second promise is resolved only once the first has been fulfilled or rejected. Consider this example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);
}).then(function() {
    console.log("Finished");
});
```

The code outputs:

```
42
Finished
```

The call to `p1.then()` returns a second promise on which `then()` is called. The second `then()` fulfillment handler is only called after the first promise has been resolved.

### Returning values in promise chains

You can continue passing data along a chain by specifying a return value from the fulfillment handler. For example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    console.log(value);         // "43"
});
```

You could do the same thing with the **rejection handler.** When a rejection handler is called, it may return a value. If it does, that value is used to fulfill the next promise in the chain, like this:

```js
let p1 = new Promise(function(resolve, reject) {
    reject(42);
});

p1.catch(function(value) {
    // first fulfillment handler
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    // second fulfillment handler
    console.log(value);         // "43"
});
```

## Promise.all()

The `Promise.all()` method accepts a single argument, which is an iterable (such as an array) of promises to monitor, and returns a promise that is resolved only when every promise in the iterable is resolved. The returned promise is fulfilled when every promise in the iterable is fulfilled, as in this example:

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.then(function(value) {
    console.log(Array.isArray(value));  // true
    console.log(value[0]);              // 42
    console.log(value[1]);              // 43
    console.log(value[2]);              // 44
});
```

Each promise here resolves with a number. The call to `Promise.all()` creates promise `p4`, which is ultimately fulfilled when promises `p1`, `p2`, and `p3` are fulfilled. The result passed to the fulfillment handler for `p4` is an array containing each resolved value: 42, 43, and 44. The values are stored in the order the promises resolved, so you can match promise results to the promises that resolved to them.

If any promise passed to `Promise.all()` is rejected, the returned promise is immediately rejected without waiting for the other promises to complete.

```js
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.catch(function(value) {
    console.log(Array.isArray(value))   // false
    console.log(value);                 // 43
});
```

In this example, `p2` is rejected with a value of 43. The rejection handler for `p4` is called immediately without waiting for `p1` or `p3` to finish executing (They do still finish executing; `p4` just doesn't wait.)

## Things to understand to grasp Promise better

### [1]

The main point to keep in mind that the promise does not make async code to run in sync mode. It does not block event queue. The async code still execute in event loop. **The main reason to introduce promise was to make code look and manage better by get rid of callback hell.** Because callback way of handling async code makes developer life miserable when it grows because it become difficult to manage error handling and result from each callback to be used in nested callback.

```js
// ### Callback approach - To handle multiple async request
readFileContents("a.txt", function(a) {
  readFileContents("b.txt", function(b) {
    writeFileContents("ab.txt", a + b, function() {
      console.log("we are done");
    });
  });
});

// ### Promise approach - It make code look and manage better by making async requests
//look like sync request but not actually become synchronized.

// ## 1 ##
var result = [];
readFileContents("a.txt").then(function(a) {
  result.push(a);
  return readFileContents("b.txt");
}).then(function(b) {
  result.push(b);
  return writeFileContents("ab.txt", result.reduce((x, y) => x + y));
}).then(function() {
  console.log("we are done");
}).catch(function(err) {
  console.error(err);
});

// ## 2 ##
Promise.all(
  readFileContents("a.txt"),
  readFileContents("b.txt")
).then(function(values) {
  return writeFileContents("ab.txt", values.reduce((x, y) => x + y));
}).then(function() {
  console.log("we are done");
}).catch(function(err) {
  console.error(err);
});
```

### [2]

Promise object has two methods `then` and `catch`. The object that is created by class `Promise`.

### [3]

`Promise` class has three static methods `all`, `resolve`, `reject` and `race`. These static methods returns Promise's object.

### [4] `Promise.resolve()`

It is a static function of Promise class.

It returns promise's object. There are few points need to grasp, see exapmle below:

```js
// ### 1. If parameter is a value, then it returns Promise's object which has two mentods
//`then` and `catch` that can be used.

var p = Promise.resolve('abhay');
p.then(function(result){
  console.log(result) // abhay
});

// can be rewrite as
Promise.resolve('abhay').then(function(result){
  console.log(result) // abhay
});

// ## 2. If parameter is a another promise's object, then `resolve` returns the promise's object
//passed in parameter.

var p1 = Promise.resolve('abhay');
var p2 = Promise.resolve(p1);

console.log(p1 === p2) // true, because second promise retruns promise's object `p1`

p2.then(function(result){
  console.log(result) // abhay
});
p1.then(function(result){
  console.log(result) // abhay
});

// ## If parameter is thenable object then `resolve` cast(convert) it into promise's object

var p1 = Promise.resolve({
  then: function (onFulfill, onReject) {
    onFulfill('fulfilled!');
  }
});
console.log(p1 instanceof Promise) // true, object casted (converted) to a Promise's object
p1.then(function (v) {
  console.log(v); // "fulfilled!"
});

/**
* Throw error behavior in thenable object
*/
// Behavior 1
var p2 = Promise.resolve({
  then: function (onFulfill, onReject) {
    throw new TypeError('Throwing');
    onFulfill('fulfilled!');
  }
});
p2.then(function(v) {
  // not called
}, function(e) {
  console.log(e); // TypeError: Throwing
});

// Behavior 2
var p3 = Promise.resolve({
  then: function (onFulfill, onReject) {
    onFulfill('fulfilled!');
    throw new TypeError('Throwing');
  }
});
p3.then(function(v) {
  console.log(v); // "Resolving"
}, function(e) {
  // not called
});
```

### [5] `Promise.reject()`

It is a static function of Promise class.

It returns promise's object and accept any javascript's variable type. As standard it is recommended to pass Error object. A few points:

```js
// ### Always pass Error object or any javascript variable type except Promise object as we do in
// Promise.resolve(). Because Promise.reject() always creates new Peomise object it does not
// returns Promise's object as it is done in the case of Promise.resolve().

var p1 = Promise.reject(new Error('rejected'));
var p2 = Promise.reject(p1);

console.log(p1 === p2); // false

p2.catch(function(err){
  console.log(err === p1); // it is a `p1` promise's object
});
```

### [6] `promiseObject.then(onFulfilled[, onRejected])`

It returns Promise object.

The behavior of handler function (`onFulfilled` or `onRejected`) on different scenarios:

```js
// ### 1. Returns simple value then `then` returns new promise's object having resolved
// value === retuned value
var p1 = p.then(function(value){
  console.log(value); // abhay
  return value + ' ' + 'garg';
});
console.log(p === p1); // false
p1.then(function(value){
  console.log(value); // abhay garg
});

// ### 2. throw an error then `then` returns new promise's object having rejected value === thrown
// error object
var p2 = p.then(function(value){
  console.log(value); // abhay
  throw Error('error');
});
console.log(p === p2); // false
p2.then(function(value){
  console.log(value); // never called
}).catch(function(err){
  console.log(err.message); // error
});

// ### 3. Returns reolved promise then `then` returns new promise's object having resolved
// value === resolved value of returned resolved promise object
var resolvedPromiseObject = Promise.resolve('garg');
var p3 = p.then(function(value){
  console.log(value); // abhay
  return resolvedPromiseObject;
});
console.log(p === p3); // false
console.log(p3 === resolvedPromiseObject); // false
p3.then(function(value){
  console.log(value); // garg
});

// ### 4. Returns rejected promise then `then` returns new promise's object having rejected
// value === rejected value of returned rejected promise object
var rejectedPromiseObject = Promise.reject(new Error('error'));
var p4 = p.then(function(value){
  console.log(value); // abhay
  return rejectedPromiseObject;
});
console.log(p === p4); // false
console.log(p4 === rejectedPromiseObject); // false
p4.then(function(value){
  console.log(value); // never called
}).catch(function(err){
  console.log(err.message); // error
});

// ### 5. Returns pending promise object then `then` returns new promise's object which
// is reolved or rejected  on the basis of returned pending promise.
var c = true;
var p1 = new Promise(function(resolve, reject){
  if(c) {
    resolve('promise one');
  } else {
    reject('promise one error');
  }
});

var p2 = new Promise(function(resolve, reject){
  if(c) {
    resolve('promise two');
  } else {
    reject('promise two error');
  }
});

var p5 = p1.then(function(value){
  console.log(value); // promise one
  return p2;
});
console.log(p5 === p2); // false
p5.then(function(value){
  console.log(value); // promise two
}).catch(function(err){
  console.log(err);
});
```

> The reason `then` returns new promise's object always so that chainable mechanism can be implemented

### [7] Do not nest one promise in to another. Use`then` method to use another pending promise.

> **One Promise === One Responsibility**

```js
// ### Still gives an accurate result but very confusing on how things would actually executes.
// Not Recommended.
var c = true;
var p1 = new Promise(function(resolve, reject){
  if(c) {
    resolve('abhay');
  } else {
    reject('error from p1');
  }
});

var p2 = new Promise(function(resolve, reject){
  // some code
  var l = 'garg';
  p1.then(function(value){
    resolve(value + ' ' + l);
  }).catch(function(err){
    resolve(err);
  });
});

p2.then(function(value){
  console.log(value); // abhay garg
}).catch(function(err){
  console.log(err);
});

// ### Recommneded
var c = true;
var p1 = new Promise(function(resolve, reject){
  if(c) {
    resolve('abhay');
  } else {
    reject('error from p1');
  }
});

var d = true;
var p2 = new Promise(function(resolve, reject){
  if(d) {
    resolve('garg');
  } else {
    reject('error from p2');
  }
});

var result = [];
p1.then(function(value){
  result.push(value);
  return p2;
}).then(function(value){
  result.push(value);
  console.log(result.join(' ')); // abhay garg
}).catch(function(err){
  console.log(err);
});
```
