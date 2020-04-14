# Redux Saga

`redux-saga` is a library that aims to make application side effects (i.e. asynchronous things like data fetching and impure things like accessing the browser cache) easier to manage, more efficient to execute, simple to test, and better at handling failures.

The mental model is that a saga is like a separate thread in your application that's solely responsible for side effects. `redux-saga` is a redux middleware, which means this thread can be started, paused and cancelled from the main application with normal redux actions, it has access to the full redux application state and it can dispatch redux actions as well.

It uses an ES6 feature called Generators to make those asynchronous flows easy to read, write and test.

## Why redux-saga

- **Declarative effects**: all operations in _redux-saga_ yield plain Javascript objects, which makes it easier to test the business logic.
- **Advanced async control flow** : simply describe async flow with sync style and familiar control flow constructs.
- **Concurrency management** : provide primitives and operators to manage concurrency between tasks; able to  _fork_ multiple background tasks in parallel and _cancel_ a running task.
- **Side effect handler** : all side effects are moved into sagas, UI components do not perform any business logic but only dispatch actions to notify what happened.
