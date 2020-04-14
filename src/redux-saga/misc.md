# Misc

## delay()

You can use `delay` helper to delay the action to specific `ms`.

> Retrying XHR calls

```js
import { delay } from 'redux-saga'
import { call, put, take } from 'redux-saga/effects'

function* updateApi(data) {
  for(let i = 0; i < 5; i++) {
    try {
      const apiResponse = yield call(apiRequest, { data });
      return apiResponse;
    } catch(err) {
      if(i < 4) {
        yield call(delay, 2000);
      }
    }
  }
  // attempts failed after 5 attempts
  throw new Error('API request failed');
}

export default function* updateResource() {
  while (true) {
    const { data } = yield take('UPDATE_START');
    try {
      const apiResponse = yield call(updateApi, data);
      yield put({
        type: 'UPDATE_SUCCESS',
        payload: apiResponse.body,
      });
    } catch (error) {
      yield put({
        type: 'UPDATE_ERROR',
        error
      });
    }
  }
}
```
