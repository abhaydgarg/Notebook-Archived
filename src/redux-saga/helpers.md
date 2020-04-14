# Helpers

> By using lower level api such as `take`, `fork`, `spawn`, `cancel` etc you can also create helper function for yourself like `takeEvery` and `takeLatest`.

## takeEvery(pattern, saga, ...args)

> Internally, it is build using `take` + `fork`.

Allows multiple instances of same action to be executed simultaneously. For example, if you click some button, which dispatches the `FETCH_USER` action to fetch user from API. Then on first click single instance of `FETCH_USER` action is created which fetches the user, and if you click the button again then another instance of `FETCH_USER` action is created, which also fetches the user from API.

**pattern:** `String | Array | Function`

* **String:** If called with `*` all dispatched actions are matched. Or the action is matched by `action.type === pattern` (e.g. `takeEvery(INCREMENT_ASYNC)`).
* **Array:** Each item in the array is matched. (e.g. `takeEvery([INCREMENT, DECREMENT]`) and that would match either actions of type `INCREMENT` or `DECREMENT`).
* **Function**: Matches if function return `true`.

**saga:** A Generator function.

## takeLatest(pattern, saga, ..args)

> Internally, it is build using `take` + `fork` + `cancel`.

It allows only one instance of action. As contrary to the example given in `takeEvery`, in the case of `takeLatest`. If you hit button multiple times then it do create multiple instances of action but upon creating new instance, the previous ongoing instance of action is terminated.
