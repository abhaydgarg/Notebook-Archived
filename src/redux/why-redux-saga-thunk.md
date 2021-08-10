# Why redux-thunk or redux-saga

Let's go step by step to see how many ways we can handle action's dispatch.

Example: Show and hide notification.

## [1] Inline

```js
//# Some component.
this.props.dispatch({ type: 'SHOW_NOTIFICATION', text: 'You logged in.' })
setTimeout(() => {
  this.props.dispatch({ type: 'HIDE_NOTIFICATION' })
}, 5000)
```

### Pros

- Simple and straightforward.
- No middleware required.

### Cons

- React component contains business logic, bad for reusing. It's not obvious in this example, but imagine we need to fetch data from server asynchronously.
- It forces you to duplicate this logic anywhere you want to show a notification. You are using structure `{ type: 'SHOW_NOTIFICATION', text: 'any message' }` all over the place where required.

## [2] Extracting action creator

Extract a function that centralizes the timeout logic and dispatches those two actions.

```js
//# actions.js
function showNotification(text) {
  return { type: 'SHOW_NOTIFICATION', text }
}
function hideNotification() {
  return { type: 'HIDE_NOTIFICATION' }
}

export function showNotificationWithTimeout(dispatch) {
  dispatch(showNotification(text))
  setTimeout(() => {
    dispatch(hideNotification())
  }, 5000)
}
```

Now components can use `showNotificationWithTimeout` without duplicating this logic.

```js
//# component.js
showNotificationWithTimeout(this.props.dispatch, 'You just logged in.')

//# otherComponent.js
showNotificationWithTimeout(this.props.dispatch, 'You just logged out.')
```

Why does `showNotificationWithTimeout()` accept `dispatch` as the first argument? Because it needs to dispatch actions to the store. Normally a component has access to `dispatch` but since we want an external function to take control over dispatching, we need to give it control over dispatching.

### Pros

- Logic is centralized, no more duplication.
- Component doesn't need to know business logic. You can fetch data from API in function `showNotificationWithTimeout()` in a `action.js` file.

### Cons

- We have to pass `dispatch` around which makes it trickier to separate container and presentational components.
- You can’t just bind action creators with `connect()` anymore because `showNotificationWithTimeout()` is not really an action creator. It does not return a Redux action's object.

## Middleware

For simple apps, the approach mentioned above should suffice. Don’t worry about middleware if you’re happy with it. In larger apps, however, you might find certain inconveniences around it.

For example using redux-thunk as middleware. We “taught” Redux to recognize such “special” action creators (we call them thunk action creators), we can now use them in any place where we would use regular action creators. For example, we can use them with `connect()`. Earlier we have to have object as paramter for `dispatch` function in the form `{type: 'SHOW_NOTIFICATION', text: 'message'}` but now with thunk middleware we can pass function to `dispatch` which further handle the dispatching of action.

```js
//# actions.js
function showNotification(text) {
  return { type: 'SHOW_NOTIFICATION', text }
}
function hideNotification() {
  return { type: 'HIDE_NOTIFICATION' }
}

export function showNotificationWithTimeout(text) {
  return function (dispatch) {
    dispatch(showNotification(text))

    setTimeout(() => {
      dispatch(hideNotification())
    }, 5000)
  }
}
```

```js
//# Some component
this.props.showNotificationWithTimeout('You just logged in.')

export default connect(mapStateToProps, {showNotificationWithTimeout})(MyComponent)
```
