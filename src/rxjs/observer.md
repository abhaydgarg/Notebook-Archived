# Observer

An Observer is a consumer of values delivered by an Observable.

Observers are just objects with three callbacks, one for each type of notification that an Observable may deliver.

```js
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

// To use the Observer, provide it to the observable.subscribe

observable.subscribe(observer);
```

You can provide arguments instead of observer object to `subscribe` function. Internally in `observable.subscribe`, it will create an Observer object using the first callback argument as the next handler, second as the error handler and third one as the complete handler.

```js
observable.subscribe(
    x => console.log('Observer got a next value: ' + x),
    err => console.error('Observer got an error: ' + err),
    () => console.log('Observer got a complete notification')
);
```
