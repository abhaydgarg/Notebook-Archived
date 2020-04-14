# Subject

An RxJS Subject is a special type of Observable that allows values to be multicasted to many Observers. While plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable), Subjects are multicast.

_**Every Subject is an Observable.**_ Given a Subject, you can `subscribe` to it, providing an Observer, which will start receiving values normally. From the perspective of the Observer, it cannot tell whether the Observable execution is coming from a plain unicast Observable or a Subject.

Internally to the Subject, `subscribe` does not invoke a new execution that delivers values. It simply registers the given Observer in a list of Observers, similarly to how `addListener` usually works in other libraries and languages.

_**Every Subject is an Observer.**_ It is an object with the methods next(v), error(e), and complete(). To feed a new value to the Subject, just call next(theValue), and it will be multicasted to the Observers registered to listen to the Subject.

In the example below, we have two Observers attached to a Subject, and we feed some values to the Subject:

```js
// Subject as Observable
var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(1);
subject.next(2);

/*
observerA: 1
observerB: 1
observerA: 2
observerB: 2
*/
```

Since a Subject is an Observer also, this also means you may provide a Subject as the argument to the `observable.subscribe()` of any Observable, like the example below shows:

```js
// Subject as Observer
var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

var observable = Rx.Observable.from([1, 2, 3]);

observable.subscribe(subject); // You can subscribe providing a Subject

/*
observerA: 1
observerB: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
*/
```

> With the two examples above, we essentially just converted a unicast Observable execution to multicast, through the Subject. This demonstrates how Subjects are the only way of making any Observable execution be shared to multiple Observers.

## BehaviorSubject

One of the variants of Subjects is the `BehaviorSubject`, which has a notion of **"the current value"**. It stores the latest value emitted to its consumers, and whenever a new Observer subscribes, it will immediately receive the "current value" from the `BehaviorSubject`.

```js
var subject = new Rx.BehaviorSubject(0); // 0 is the initial value

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(3);

/*
observerA: 0
observerA: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
*/
```

## ReplaySubject

A ReplaySubject is similar to a BehaviorSubject in that it can send old values to new subscribers, but it can also record a part of the Observable execution. A ReplaySubject records multiple values from the Observable execution and replays them to new subscribers.

```js
// When creating a ReplaySubject, you can specify how many values to replay
var subject = new Rx.ReplaySubject(3); // buffer 3 values for new subscribers

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(5);

/*
observerA: 1
observerA: 2
observerA: 3
observerA: 4
observerB: 2
observerB: 3
observerB: 4
observerA: 5
observerB: 5
*/
```

## AsyncSubject

The AsyncSubject is a variant where only the last value of the Observable execution is sent to its observers, and only when the execution completes.

```js
var subject = new Rx.AsyncSubject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(5);
subject.complete();

/*
observerA: 5
observerB: 5
*/

// The AsyncSubject is similar to the last() operator, in that it waits
// for the complete notification in order to deliver a single value.
```

## How subject works

> Subject is both Observable and Observer.

### Subject as Observable

You can just use `Observable.create()` or Observable creator operator like `of`, `from`, `fromPromise` etc to create Observable. The only difference between core Observable and Subject as Observable is that core Observable provide independent copy to each Observers whereas Observers listening to Subject get shared copy.

```js
// create Observable from Subject
var subject = new Subject();

// Subscribe to Observable as we do in Observable
subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

// Pass value to Observer as we do in
`Observable.create(function subscribe(observer) { observer.next(1); });`
// You can also make use of subject.error() and subject.complete()
subject.next(1);

// the value 1 will be passed to two Observers subscribed above to Subject as Observable

/*
observerA: 1
observerB: 1
*/
```

```js
//# Core Observable - Independent copy

/*
 Even though `observerB` subscribe to Observable after 3 seconds it
 still get the independent copy of `function subscribe(observer) {}` that means for each Observers `function subscribe(observer) {}` executes from start or fresh.
*/
var observable = Observable.create(function subscribe(observer) {
  observer.next(1);
  observer.next(2);
});

// subscribe immediately
observable.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
// subscribe after 3 seconds
setTimeout(function() {
  observable.subscribe({
    next: (v) => console.log('observerB: ' + v)
  });
}, 3000);

/*
observerA: 1
observerA: 2
observerB: 1
observerB: 2
*/

//# Subject - Shared copy

/*
 `observerB` subscribe to Subject Observable after 3 seconds it
 never gets the value 1.
*/

// create Observable
var subject = new Subject();

// subscribe immediately
subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
// subscribe after 3 seconds
setTimeout(function() {
  subject.subscribe({
    next: (v) => console.log('observerB: ' + v)
  });
}, 3000);

// pass value to observers immediately
subject.next(1);

// pass value to observers after 4 seconds
setTimeout(function() {
 subject.next(2);
}, 4000);

/*
observerA: 1
observerA: 2
observerB: 2
*/
```

### Subject as Observer

```js
// create core Observable `observable`
var observable = Observable.from([1, 2, 3]);

// create subject `subject`
var subject = new Subject();

/*
 Subject is Observable too, so subscribing to Subject creates internal
 list of Observers maintain by Subject internally that only listen to
 Subject Observable called `subject`.
 */
// Subscribe to `subject` and create internal list of Observers
subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

// `subject` is Observer too, so we can pass it to subscribe function of core Observable called `observable`
observable.subscribe(subject);

/*
observerA: 1
observerB: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
*/
```

Now what happened when `subject` is passed to `observable` as observer.

* `observable` pass/send value `1` to its observer called `subject`.
* `subject` receives a value `1` which also a Observable and two internal observes are attach to it.
* `subject` pass/send value `1` to its internal observers.
