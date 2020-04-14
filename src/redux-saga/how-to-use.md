# How to use

## Create watcher and worker

```js
//### userSaga.js
import { takeLatest, call, put } from 'redux-saga/effects';
import API from '../api';

// user's watchers that are registred in sagas.js
// so that redux-saga can listen to actions `USERS_REQUEST_ACTION`
// and `USER_REQUEST_ACTION`. When some component dispatches any of
// these actions then redux-saga runs the associated worker function
export default function userWatchers() {
  // Effects like `takeLatest` or `takeEvery` return
  // simple objects
  return [
    takeLatest('USERS_REQUEST_ACTION', usersWorker),
    takeLatest('USER_REQUEST_ACTION', userWorker)
  ];
}

// worker saga: makes the api call to fetch users
// and handle success, failure action
function* usersWorker() {
  try {
    const response = yield call(API.fetchUsers);
    // dispatch a success action
    yield put({ type: 'USERS_SUCCESS_ACTION', response });

  } catch (error) {
    // dispatch a failure action
    yield put({ type: 'USERS_FAILURE_ACTION', error });
  }
}

// worker saga: makes the api call to fetch single user
// and handle success, failure action
function* userWorker() {
  try {
    const response = yield call(API.fetchUser);
    // dispatch a success action
    yield put({ type: 'USER_SUCCESS_ACTION', response });

  } catch (error) {
    // dispatch a failure action
    yield put({ type: 'USER_FAILURE_ACTION', error });
  }
}
```

## Register watchers

```js
//### sagas.js
import userWatchers from './userSaga';
//... import other sagas

// Root watcher saga: Register your app watchers here
export function* watcherSaga() {
  // combine all watchers first
  const allWatchers = [
    ...userWatchers()
  ];
  yield all(allWatchers);
}
```

## Register middleware

```js
//### index.js - Register the redux-saga middleware to redux store
mport { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';

import reducer from './reducers';
import sagas from './sagas';

// create the saga middleware
const sagaMiddleware = createSagaMiddleware();
// mount it on the Store
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)

// then run the saga
sagaMiddleware.run(sagas);
```

## Call saga

```js
//### User.js - Component
import React, { Component } from 'react';
import { connect } from 'react-redux';

class User extends Component {
  //...
}

const mapDispatchToProps = dispatch => {
  return {
    // dispatch action to store
    // and run saga
    fetchUsers: () => dispatch({ type: 'USERS_REQUEST_ACTION' }),
    fetchUser: () => dispatch({ type: 'USER_REQUEST_ACTION' })
  };
};

export default connect(null, mapDispatchToProps)(User);
```
