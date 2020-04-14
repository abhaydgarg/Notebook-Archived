# Effect combinators

Let you run multiple effects in one go.

## all([...effects]) - parallel effects

> Run multiple Effects in parallel and wait for all of them to complete. Like `Promise#all`.

```js
// Effects will be executed in sequence, one by one
// Since `call` is a blocking effect, so unless
// first statement is completed, the second won't
// be executed.
const customers = yield call(fetchCustomers);
const products = yield call(fetchProducts);
```

```js
const [customers, products] = yield all([
    call(fetchCustomers),
    call(fetchProducts)
  ]);

// OR

const { customers, products } = yield all({
    customers: call(fetchCustomers),
    products: call(fetchProducts)
  });
```

> All other effects terminated if any one of them rejected.

## race(effects)

> Same as `all`, multiple effects run in parallel, but does not wait for all effects to finish rather whichever finishes first it returns the result of winner and terminates all other effects. It is just interested in winner.

`race` combinator can be used, for example in code below, where we have established time constraint. If `call(fetchApi, '/posts')` fetches posts within 1 second then you will get `posts` data otherwise `call(delay, 1000)` will finish first and which result in running `put({type: 'TIMEOUT_ERROR'});` statement.

```js
import { race, take, put } from 'redux-saga/effects';
import { delay } from 'redux-saga';

function* fetchPostsWithTimeout() {
  const {posts, timeout} = yield race({
    posts: call(fetchApi, '/posts'),
    timeout: call(delay, 1000)
  });

  if (posts)
    put({type: 'POSTS_RECEIVED', posts});
  else
    put({type: 'TIMEOUT_ERROR'});
}
```
