# Points to note

## Compose saga with extra layer of yield using generator function

Look at the code below.

```js
// foo.js
export function* fooSagas() {
  yield all([
    takeEvery("FOO_A", fooASaga),
    takeEvery("FOO_B", fooBSaga),
  ]);
}

// bar.js
export function* barSagas() {
  yield all([
    takeEvery("BAR_A", barASaga),
    takeEvery("BAR_B", barBSaga),
  ]);
}

// index.js
import { fooSagas } from './foo';
import { barSagas } from './bar';

export default function* rootSaga() {
  yield all([
    fooSagas,
    barSagas
  ])
}
```

It is okay to compose saga this way. It would definitely work. But this approach uses more memory and become less performant. Each saga - what you pass to the `sagaMiddleware.run/runSaga` and every generator passed into the call, apply, fork, spawn, all, race effects is internally becoming a Task. So if you add another layer of the generators in between they all get instances internally etc. Like in example above, it is unnecessary to use generator function for `fooSagas`, `barSagas` and use `all` to run effects. Lets see how, when you run `fooSagas` in `rootSaga()` using `all` then first task is created to handle `fooSagas` generator function and in next step second task is created for handling `fooSagas` body's content which is `yield all()`. So in total you have two `yield all()` to run effect `takeEvery("FOO_A", fooASaga)`. It takes two steps for middleware to run the effect.

This can be done in one step because as every effect is just an object you can actually export them as object only instead of the generators. See better version of above code:

```js
// foo.js
export const fooSagas = [
  takeEvery("FOO_A", fooASaga),
  takeEvery("FOO_B", fooBSaga),
]

// bar.js
export const barSagas = [
  takeEvery("BAR_A", barASaga),
  takeEvery("BAR_B", barBSaga),
];

// index.js
import { fooSagas } from './foo';
import { barSagas } from './bar';

export default function* rootSaga() {
  yield all([
    ...fooSagas,
    ...barSagas
  ])
}
```
