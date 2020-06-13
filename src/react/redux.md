# Redux

## action

```js
export const ADD_NOTE = 'ADD_NOTE';

export function addNote(title, content) {
  return { type: ADD_NOTE, title: title, content: content };
}
```

- We are exporting a constant `ADD_NOTE`, since we’re going to be needing it in several places later on.
- We are exporting the function `addNote`. This function is an **action creator**, which means that its job is to only return a plain object.

## reducer

> (previousState, action) => newState

It takes two arguments: the previous app state, the action being dispatched and returns the new app state.

```js
import { ADD_NOTE } from './actions';

const initialState = {
  notes: []
};

function rootReducer(state = initialState, action) {
  switch(action.type) {
    case ADD_NOTE:
      return {
        notes: [
          ...state.notes,
          {
            title: action.title,
            content: action.content
          }
        ]
      };
    default:
      return state;
  };
}

export default rootReducer;
```

## mapStateToProps

> If you provide mapStateToProps then only the component is subscribed to store - re-render when state's object get changed. It listen to state's changes.

| Subscribe                                               | Not subscribe                                |
|---------------------------------------------------------|----------------------------------------------|
| connect(mapStateToProps)(Component)                     | connect()(Component)                         |
| connect(mapStateToProps, mapDispatchToProps)(Component) | connect(null, mapDispatchToProps)(Component) |

```js
class Notes extends Component {
  render() {
    console.log(this.props.notes);
  }
}

const mapStateToProps = state => {
  return {
    notes: state.notes
  };
};

export default connect(mapStateToProps)(Notes);
```

`mapStateToProps` takes an **optional second argument**, which lets you use the component own props that are not from store.

```js
const mapStateToProps = (state, ownProps) => {
  return {
    notes: state.notes
  };
};
```

### Selectors

A selector is simply this: A function that takes the current application state and returns the relevant portion needed by the view.

For example:

```javascript
// This is what we usually do to get
// state and map to props. We are doing
// filter operation on state before mapping
// to props. This may lead to messy mapStateToProps
// function.
function mapStateToProps(state) {
  return {
    incompleteTodos: state.todos.filter((todo) => {
      return !todo.completed
    });
  }
}
```

Instead, we can create selector function in reducer file:

```javascript
// Selector
function getIncompleteTodos(state) {
  return state.todos.filter((todo) => {
    return !todo.completed
  });
}
```

Now, we could use the newly created selector in its place:

```javascript
function mapStateToProps(state) {
  return {
    incompleteTodos: getIncompleteTodos(state)
  };
}
```

The code is more concise and we have made a big step toward encapsulating our state tree. The component no longer cares how todos are stored in the state tree and it will be much easier to refactor if the state tree needs to be updated!

### Performance of mapStateToProps

#### It should be fast

!!! important "Why"
    Whenever the store changes, all of the `mapStateToProps` functions of all of the connected components will run. This is very important to understand. This means, say, if you have 10 comps connected (subscribed) to store by `mapStateToProps` and store's state get changed. Now react-redux does not know which comp is acually using the property that has changed. So react-redux runs `mapStateToProps` function of all 10 comps and compare the old and new values of `mapStateToProps` if it sees the change in any of the value then it re-render the comp or otherwise skip the render. Because of this, your `mapStateToProps` function should run as fast as possible.

##### How react-redux ensures the performance of mapStateToProps

`connect` is a HOC comp, which uses the `shouldComponentUpdate` method internally and when `mapStateToProps` function is executted. React Redux decides whether the contents of the object returned from `mapStateToProps` are different from previous using `===` comparison (a "shallow equality" check) on each fields of the returned object. If any of the fields have changed, then your component will be re-rendered so it can receive the updated values as props.

> This way react-redux avoid the reconciliation process _(skip running render method, so no need to compare old virtual DOM with new one and mutate the DOM if found any change)_ which is a huge benefit for performance.

##### Developer duty

> **Note:** Return Value of `mapStateToProps` determine if component re-renders.

React Redux does shallow comparisons to see if the mapStateToProps results have changed. It’s easy to accidentally return new object or array references every time, which would cause your component to re-render even if the data is actually the same.

As a developer, you should be careful about the **selector** function which returns new object or an array. In case of primitive types, you need not to worry. But be careful, when returning object or array from selector function. For example,

```js
// Selector function
function getIncompleteTodos(state) {
  return state.todos.filter((todo) => {
    return !todo.completed
  });
}

// In component
function mapStateToProps(state) {
  return {
    incompleteTodos: getIncompleteTodos(state)
  };
}
```

`incompleteTodos` will get new array always because `filter` function returns a new array. Let's say, state get changed, and react-redux run all `mapStateToProps` functions. Say the state property that has changed is not subscribed to this particular comp which is `todos`. But still react-redux sees that `incompleteTodos` array reference is different from old one. And it re-render the comp which we do not need at all.

!!! question "How to avoid re-render in case of new object or array reference"
    Put these operations in memoized selector functions (**reselect** library) to ensure that they only run if the input values have changed. This will also ensure that if the input values haven't changed, mapStateToProps will still return the same result values as before, and connect can skip re-rendering.

## mapDispatchToProps

```js
import { addNote } from './actions';

class NotesForm extends Component {
  handleSubmission = (event) => {
    // dispatch action
    this.props.addNote(title, content);
  }
}

const mapDispatchToProps = {
  addNote: addNote
};

export default connect(null, mapDispatchToProps)(NotesForm);
```

### Number of ways to dispatch

#### Default: dispatch as a Prop

If you don't specify the second argument `mapDispatchToProps`, your component will receive `props.dispatch` as prop by default.

```js
import { addNote } from './actions';

class NotesForm extends Component {

  handleSubmission = (event) => {
    // dispatch action
    this.props.dispatch(addNote(title, content));
  }
}

export default connect(null, null)(NotesForm);
```

#### Using mapDispatchToProps

If you define your own mapDispatchToProps, the connected component will no longer receive dispatch.

It lets you provide action dispatching functions as props. Therefore, instead of calling `this.props.dispatch(() => addNote())`, you may call `this.props.addNote()` directly. Having dispatch function as prop you can **pass it down to child** component as prop. So child comp need not be aware of redux and requir to be connected using `connect()` and still dispatch an action.

##### [1] mapDispatchToProps as a function

Allows more customization.

- Arguments: `dispatch` and optional `ownProps`.
- Return: plain object where each key will become a prop of a component, and the value should normally be a function that dispatches an action when called.

```js
import { addNote } from './actions';

class NotesForm extends Component {

  handleSubmission = (event) => {
    // dispatch action
    this.props.addNote(title, content);
  }
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    addNote: (title, content) => dispatch(addNote(title, content))
  };
}

export default connect(null, mapDispatchToProps)(NotesForm);
```

##### [2] mapDispatchToProps as a object

> We recommend always using the “object shorthand” form of mapDispatchToProps, unless you have a specific reason to customize the dispatching behavior.

Setup for dispatching Redux actions in a React component follows a very similar process: define an action creator, wrap it in another function that looks like `(…args) => dispatch(actionCreator(…args))`, and pass that wrapper function as a prop to your component.

Because this is so common, connect supports an “object shorthand” form for the mapDispatchToProps argument: if you pass an object full of action creators instead of a function, `connect` will automatically call `bindActionCreators` for you internally.

**Note:** Each key of the mapDispatchToProps object is an action creator.

```js
import { addNote } from './actions';

class NotesForm extends Component {

  handleSubmission = (event) => {
    // dispatch action
    this.props.addNote(title, content);
  }
}

const mapDispatchToProps = {
  addNote: addNote
};

export default connect(null, mapDispatchToProps)(NotesForm);
```

## Why redux-thunk or redux-saga

Let's go step by step to see how many ways we can handle action's dispatch.

Example: Show and hide notification.

### [1] Inline

```js
//# Some component.
this.props.dispatch({ type: 'SHOW_NOTIFICATION', text: 'You logged in.' })
setTimeout(() => {
  this.props.dispatch({ type: 'HIDE_NOTIFICATION' })
}, 5000)
```

#### Pros

- Simple and straightforward.
- No middleware required.

#### Cons

- React component contains business logic, bad for reusing. It's not obvious in this example, but imagine we need to fetch data from server asynchronously.
- It forces you to duplicate this logic anywhere you want to show a notification. You are using structure `{ type: 'SHOW_NOTIFICATION', text: 'any message' }` all over the place where required.

### [2] Extracting action creator

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

#### Pros

- Logic is centralized, no more duplication.
- Component doesn't need to know business logic. You can fetch data from API in function `showNotificationWithTimeout()` in a `action.js` file.

#### Cons

- We have to pass `dispatch` around which makes it trickier to separate container and presentational components.
- You can’t just bind action creators with `connect()` anymore because `showNotificationWithTimeout()` is not really an action creator. It does not return a Redux action's object.

### Middleware

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

## Immutable update patterns

### Updating nested objects

The key to updating nested data is that every level of nesting must be copied and updated appropriately.

#### Common mistake #1: New variables that point to the same objects

Defining a new variable does not create a new actual object - it only creates another reference to the same object. This function does correctly return a shallow copy of the top-level state object, but because the `nestedState` variable was still pointing at the existing object, the state was directly mutated.

```js
function updateNestedState(state, action) {
  let nestedState = state.nestedState
  // ERROR: this directly modifies the existing
  // object reference - don't do this!
  nestedState.nestedField = action.data

  return {
    ...state,
    nestedState
  }
}
```

#### Common mistake #2: Only making a shallow copy of one level

Doing a shallow copy of the top level is _not_ sufficient - the `nestedState` object should be copied as well.

```js
function updateNestedState(state, action) {
  // Problem: this only does a shallow copy!
  let newState = { ...state }

  // ERROR: nestedState is still the same object!
  newState.nestedState.nestedField = action.data

  return newState
}
```

#### Correct approach: Copying all levels of nested data

Obviously, each layer of nesting makes this harder to read, and gives more chances to make mistakes.

```js
function updateVeryNestedField(state, action) {
  return {
    ...state,
    first: {
      ...state.first,
      second: {
        ...state.first.second,
        [action.someId]: {
          ...state.first.second[action.someId],
          fourth: action.someValue
        }
      }
    }
  }
}
```

### Inserting and removing items in arrays

Normally, a Javascript array's contents are modified using mutative functions like `push`, `unshift`, and `splice`. Since we don't want to mutate state directly in reducers, those should normally be avoided. Because of that, you might see "insert" or "remove" behavior written like this:

```js
function insertItem(array, action) {
  return [
    ...array.slice(0, action.index),
    action.item,
    ...array.slice(action.index)
  ]
}

function removeItem(array, action) {
  return [...array.slice(0, action.index), ...array.slice(action.index + 1)]
}
```

### Updating an item in an array

Updating one item in an array can be accomplished by using `Array.map`, returning a new value for the item we want to update, and returning the existing values for all other items:

```js
function updateObjectInArray(array, action) {
  return array.map((item, index) => {
    if (index !== action.index) {
      // This isn't the item we care about - keep it as-is
      return item
    }

    // Otherwise, this is the one we want - return an updated value
    return {
      ...item,
      ...action.item
    }
  })
}
```

### Immutable update utility libraries

- Immutable.js
- seamless-immutable
- immer

### Handling action creators, reducers, selectors efficiently

!!! info "Reduxsauce"
    This is the library that you should use for creating highly readable redux code, and much easier to maintain redux code in your app.
