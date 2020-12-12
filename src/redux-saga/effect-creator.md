# Effect

Effects in `redux-saga` are represented by an simple object, which contain the information about the effect. So when you create or call the effect's function say:

```js
const products = yield call(Api.fetch, '/products');
console.log(products);

// `products` gives you the effect's description
// which is then run by middleware.
// {
//   CALL: {
//     fn: Api.fetch,
//     args: ['./products']
//   }
// }
```

## How returning the simple object which describe the effect, help in testing

```js
// Generator function (Worker saga) to fetch products
// using effect creator function `call`.
import { call } from 'redux-saga/effects'

function* fetchProducts() {
  // the `call` creates a description of the effect.
  const products = yield call(Api.fetch, '/products')
  // ...
}
```

```js
// Testing `fetchProducts` generator function
// that fetches the products from API.
import { call } from 'redux-saga/effects';
import Api from '...';

const iterator = fetchProducts();

// expects a call instruction
assert.deepEqual(
  iterator.next().value,
  call(Api.fetch, '/products'),
  "fetchProducts should yield an Effect call(Api.fetch, './products')"
)
```

Executing the real service during tests is neither a viable nor practical approach, so we have to mock the `Api.fetch` function, i.e. we'll have to replace the real function with a fake one which doesn't actually run the AJAX request but only checks that we've called `Api.fetch` with the right arguments ('/products' in our case). What we actually need is just to make sure the `fetchProducts` task yields a call with the right function and the right arguments.

Instead of invoking the asynchronous function directly from inside the Generator, we can yield only a description of the function invocation. i.e. We'll simply yield an object which looks like

```js
// Effect -> call the function Api.fetch with `./products` as argument
{
  CALL: {
    fn: Api.fetch,
    args: ['./products']
  }
}
```

Put another way, the Generator will yield plain Objects containing instructions, and the `redux-saga` middleware will take care of executing those instructions and giving back the result of their execution to the Generator. This way, when testing the Generator, all we need to do is to check that it yields the expected instruction by doing a simple deepEqual on the yielded Object.

## Effect creators

> Each function below returns a plain JavaScript object and does not perform any execution.
> The execution is performed by the middleware during the Iteration process. The middleware examines each Effect description and performs the appropriate action.

### What is blocking effect and non blocking effect

Effect which suspends the generator function and wait for the result to get is called **blocking effect**, whereas effect which does not suspend generator function and move to next statement immediately is called **non blocking effect**.

!!! Example
    - put(action) - Non-blocking
    - put.resolve(action) - Blocking

### take(pattern)

> Suspend the generator until matching action is dispatched - **Blocking**

Creates an Effect description that instructs the middleware to wait for a specified action on the Store. The Generator is suspended until an action that matches pattern is dispatched.

Let see how something written using `takeEvery` can also be written in `take`.

```js
//### Using takeEvery
import { select, takeEvery } from 'redux-saga/effects';

function* worker(action) {
  const state = yield select();
  console.log('action', action);
  console.log('state after', state);
}

function* watcher() {
  yield takeEvery('SOME_ACTION', worker);
}
```

```js
//### Using take
import { select, take } from 'redux-saga/effects';

// Note how we're running an endless loop while (true).
// Remember, this is a Generator function, which doesn't have a
// run-to-completion behavior. Our Generator will block on each
// iteration waiting for an action to happen.
function* worker() {
  // We need while endless loop because
  // we want redux saga to listen to action
  // mutiple times. After listening for first time
  // the control again goes back to `take` for
  // future action.
  while (true) {
    const action = yield take('SOME_ACTION');
    const state = yield select();

    console.log('action', action);
    console.log('state after', state);
  }
}

function* watcher() {
  yield worker();
}
```

**Using `take`, you can have more control over the flow of saga.** You can choose how many times you want to handle specific action.

As a simple example, suppose that in our Todo application, we want to watch user actions and show a congratulation message after the user has created his first three todos.

```js
import { take, put } from 'redux-saga/effects';

function* watchFirstThreeTodosCreation() {
  for (let i = 0; i < 3; i++) {
    const action = yield take('TODO_CREATED');
  }
  yield put({type: 'SHOW_CONGRATULATION'});
}

function* watcher() {
  yield watchFirstThreeTodosCreation();
}
```

Instead of a `while (true)`, we're running a `for` loop, which will iterate only three times. After taking the first three `TODO_CREATED` actions, `watchFirstThreeTodosCreation` will cause the application to display a congratulation message then terminate. This means the Generator will be garbage collected and no more observation will take place.

### fork(fn, ...args)

> Like a `call` which execute the `fn` but **Non blocking**

#### Note

* All forked (child) tasks are attached to parent (function which invoke `fork`). When the parent terminates the execution of its own body of instructions, it will wait for all forked tasks to terminate before returning.
* If any child forked task raises an uncaught error, then the parent task will abort with the child Error, and the whole Parent's execution tree (i.e. forked (child) tasks + the main (parent) task represented by the parent's body if it's still running) will be cancelled.
* Cancellation of any forked (child) Task will automatically cancel all forked tasks that are still executing.

#### call vs. fork

`call` is blocking and `fork` is non blocking. Look at the example below:

```js
function* worker() {
  // execute and then generator is suspended
  // untill get the posts
  const posts = yield call(fetchApi, '/posts');
  // now execute and then generator is suspended
  // untill get the comments
  const comments = yield call(fetchApi, '/comments');

  console.log(posts);
  console.log(comments)
}

//### fork
function* worker() {
  // execute and move to next statement immediatelly
  // no waiting for result from api
  const posts = yield fork(fetchApi, '/posts');
  // execute and move to next statement immediatelly
  // no waiting for result from api
  const comments = yield fork(fetchApi, '/comments');

  // not get actual post and comment but the
  // Effect's description object in `post` and
  // `comment` variables.
  console.log(posts);
  console.log(comments)
}
```

### spawn(fn, ...args)

> Same as `fork` but creates a detached task. Which are independent of parent which invoke them.

* The parent will not wait for detached tasks to terminate before returning as in `fork`.
* Child tasks are completely independents of handling error and cancellation.

#### fork vs. spawn

`fork` and `spawn` will both return Task objects. Forked tasks are attached to parent, whereas spawned tasks are detached from the parent.
If you see how errors are handled in `fork` and `spawn`, you can make better choices. Let se how:

In `fork` if any child task raise an error then that error is bubbled up to parent and also it teminates the execution of all other childrens. Where as in case of `spawn`, if any of the child raises an error then other children remain unaffected. It is the duty of child task to handle error on its own than bubble to parent.

Based on above, you could, use `fork` for "mission critical" tasks, i.e. "if this task fails, please crash the whole app", and spawn for "not critical" tasks, i.e. "if this task fails, do not propagate the error to the parent" and let the app keep running.
