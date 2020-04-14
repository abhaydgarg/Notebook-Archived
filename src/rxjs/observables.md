# Observable

Observables are lazy Push collections of multiple values.

The following is an Observable that pushes the values 1, 2, 3 immediately (synchronously) when subscribed, and the value 4 after one second has passed since the subscribe call, then completes:

```js
var observable = Rx.Observable.create(function (observer) {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  setTimeout(() => {
    observer.next(4);
    observer.complete();
  }, 1000);
});

// need to use `subscribe` to invoke observable

console.log('just before subscribe');
observable.subscribe({
  next: x => console.log('got value ' + x),
  error: err => console.error('something wrong occurred: ' + err),
  complete: () => console.log('done'),
});
console.log('just after subscribe');

/*
just before subscribe
got value 1
got value 2
got value 3
just after subscribe
got value 4
done
*/
```

> Observables are like functions with zero arguments, but generalize those to allow multiple values.

```js
// give me one value synchronously
function foo() {
  console.log('Hello');
  return 42;
  return 43; // will not execute
}

var x = foo.call(); // same as foo()
console.log(x);

/*
"Hello"
42
*/

//# Using observable - rewrite above function
// Observables can "return" multiple values over time, something which functions cannot.

var foo = Rx.Observable.create(function (observer) {
  console.log('Hello');
  observer.next(42); // return 42
  observer.next(43); // then return 43
});

foo.subscribe(function (x) {
  console.log(x);
});

/*
"Hello"
42
43
*/
```

## How to create observable

There are 4 core aspect when creating observable.

### 1. Creating Observables

`Rx.Observable.create` is an alias for the Observable constructor, and it takes one argument: the `subscribe` function.

```js
// creates an Observable to emit the string 'hi' every second to an Observer.
var observable = Rx.Observable.create(function subscribe(observer) {
  var id = setInterval(() => {
    observer.next('hi')
  }, 1000);
});
```

> Observables can be created with create, but usually we use the so-called creation operators, like of, from, interval, etc.

## 2. Subscribing to Observables

```js
observable.subscribe(x => console.log(x));
```

> Subscribing to an Observable is like calling a function, providing callbacks where the data will be delivered to. Do not confuse it with listener which is registered as listener to observable. It is simply a way (when called) to start an "Observable execution" and deliver values to an Observer.

`observable.subscribe`** and subscribe in **`Observable.create(function subscribe(observer) {...})`** have the same name.**

In the library, they are different. When calling `observable.subscribe` with an Observer, the function `subscribe` in `Observable.create(function subscribe(observer) {...})` is run for that given Observer. Each call to `observable.subscribe` triggers its own independent setup for that given Observer.

## 3. Executing Observables

The code inside `Observable.create(function subscribe(observer) {...})` represents an "Observable execution", a lazy computation that only happens for each Observer that subscribes. The execution produces multiple values over time, either synchronously or asynchronously.

There are three types of values an Observable Execution can deliver:

1. "Next" notification: sends a value such as a Number, a String, an Object, etc.
2. "Error" notification: sends a JavaScript Error or exception.
3. "Complete" notification: does not send a value.

Next notifications are the most important and most common type: they represent actual data being delivered to an Observer. Error and Complete notifications may happen only once during the Observable Execution, and there can only be either one of them.

> _**Rule:**_ In an Observable Execution, zero to infinite Next notifications may be delivered. If either an Error or Complete notification is delivered, then nothing else can be delivered afterwards.

```js
var observable = Rx.Observable.create(function subscribe(observer) {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
  observer.next(4); // Is not delivered because it would violate the rule.
});
```

## 4. Disposing Observable Executions

Because Observable Executions may be infinite, and it's common for an Observer to want abort execution in finite time, we need an API for canceling an execution. Since each execution is exclusive to one Observer only, once the Observer is done receiving values, it has to have a way to stop the execution, in order to avoid wasting computation power or memory resources.

```js
// When you subscribe, you get back a Subscription, which represents the ongoing execution.
// Just call unsubscribe() to cancel the execution.
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
// Later:
subscription.unsubscribe();
```

**Provide **`unsubscribe`** function when creating Observable.**

```js
// this is how we clear an interval execution set with setInterval:
var observable = Rx.Observable.create(function subscribe(observer) {
  // Keep track of the interval resource
  var intervalID = setInterval(() => {
    observer.next('hi');
  }, 1000);

  // Provide a way of canceling and disposing the interval resource
  return function unsubscribe() {
    clearInterval(intervalID);
  };
});
```
