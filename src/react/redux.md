# Redux

![Redux](assets/1_Xr2pZxU0OqIO4cABIXml8g.png)

- As the application grows bigger, managing states shared across components becomes a difficult and hard to debug. Basically, the state will have to be lifted up to the nearest parent component and to the next until it gets to an ancestor that is common to both components that need the state and then it is passed down. This makes the state difficult to maintain and less predictable. It also means passing data to components (intermediate comps - props drilling) that do not need such data.
- React pass data top to down and not across comps. Using redux we can pass data to any comp anywhere in the app.
- There is a central **store** that holds the entire state of the application. Each component can access the stored state without having to send down props from one component to another.
- **Actions** are events. They are the only way you can send data from your application to your Redux store. Actions are plain JavaScript objects and they must have a type property to indicate the type of action to be carried out. They must also have a payload that contains the information that should be worked on by the action.
- **Reducers** are pure functions that take the current state of an application, perform an action and returns a new state.
- Easy to debug an application because there is single source of truth of state. App has one central unit called store which holds app's state. Reducers are where the state is updated and no where else. So developer knows where to look when things go wrong while updating state.
- Easy to test because reducers are pure function and we need to test the reducer only which is responsible for state changes.

## Code

### action.js

```js
export const ADD_NOTE = 'ADD_NOTE';

export function addNote(title, content) {
  return { type: ADD_NOTE, title: title, content: content };
}
```

- We are exporting a constant `ADD_NOTE`, since we’re going to be needing it in several places later on.
- We are exporting the function `addNote`. This function is an **action creator**, which means that its job is to only return a plain object.

### reducers.js

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

### Notes component

> Access notes state from store.

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

`mapStateToProps` is actually a function which takes the **entire state** of our app as its first argument and returns an object of data as props that our component will need.

`mapStateToProps` takes an **optional second argument**, which lets you use the component own props that are not from store.

```js
const mapStateToProps = (state, ownProps) => {
  return {
    notes: state.notes
  };
};
```

### NoteForm component

> Dispatch action

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

#### Number of ways to dispatch

##### Default: dispatch as a Prop

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

##### Using mapDispatchToProps

If you define your own mapDispatchToProps, the connected component will no longer receive dispatch.

It lets you provide action dispatching functions as props. Therefore, instead of calling `this.props.dispatch(() => addNote())`, you may call `this.props.addNote()` directly. Having dispatch function as prop you can **pass it down to child** component as prop. So child comp need not be aware of redux and requir to be connected using `connect()` and still dispatch an action.

###### [1] mapDispatchToProps as a function

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

###### [1] mapDispatchToProps as a object

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

## Three Principles

### Single source of truth

The state of your whole application is stored in an object tree within a single store.

### State is read-only

The only way to change the state is to emit an action. This ensures that neither the views nor the network callbacks will ever write directly to the state. Instead, they express an intent to transform the state by dispatching action.

### Changes are made with pure functions

To specify how the state tree is transformed by actions, you write pure reducers.

Reducers are just pure functions that take the previous state and an action, and return the next state. Remember to return new state objects, instead of mutating the previous state.

1. Given the same input, always returns the same output.
2. Has no side-effects.

Importantly in JavaScript, all non-primitive objects are passed into functions as references. In other words, if you pass in an object, and then directly mutate a property on that object, the object changes outside the function as well. That’s a side-effect.

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

Because writing immutable update code can become tedious, there are a number of utility libraries that try to abstract out the process. These libraries vary in APIs and usage, but all try to provide a shorter and more succinct way of writing these updates. For example, Immer makes immutable updates a simple function and plain JavaScript objects.

!!! note "Immutable data structures vs Immutable update utility"
    Immutable data structures library such as Immutable.js and seamless-immutable freeze the state and if you try to update it then it generate an error.

    where as

    Immutable update utility library such as immer help in creating next state (cloning) without accidental mutation to original state. Traditional way of running update in reducers using `...` where we have nested objects are tedious and error-prone.

### Simplifying immutable updates with redux toolkit

Redux Toolkit package includes a `createReducer` utility that uses Immer internally. Because of this, you can write reducers that appear to "mutate" state, but the updates are actually applied immutably.

This allows immutable update logic to be written in a much simpler way. Here's what the nested data example might look like using `createReducer`:

```js
import { createReducer } from '@reduxjs/toolkit'

const initialState = {
  first: {
    second: {
      id1: { fourth: 'a' },
      id2: { fourth: 'b' }
    }
  }
}

const reducer = createReducer(initialState, {
  UPDATE_ITEM: (state, action) => {
    state.first.second[action.someId].fourth = action.someValue
  }
})
```

This is clearly much shorter and easier to read. However, this only works correctly if you are using the "magic" createReducer function from Redux Toolkit that wraps this reducer in Immer's produce function.

## Data subscribing to trigger re-render

!!! question
    I want to keep some data in the redux store that does not affect React components at all. If i update the data which is not used in any component then does React run the update mechanism or not?

Updating data in redux will only trigger a re-render to a component if the data that specific component is "subscribing" to has changed.

And the component subscribe to the data by `mapStateToProps`. For example:

```javascript
const mapStateToProps = state => {
  return {
    cars: state.cars,
  }
}
```

Now, this component is subscribe to `state.cars`, so if you change the `state.cars` then only the componenet will re-render.

Now, if you have a piece of data in the store which is not defined in any `mapStateToProps`, no component will render as as result of changing that data point.

## Selectors

Selectors are not technically part of Redux itself. A selector is simply this: A function that takes the current application state and returns the relevant portion needed by the view.

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

### One step further (memoized selectors)

Let’s say you have a component that needs to run an intensive sorting operation on the store’s state in order to get the data it needs. It would be great if we could only run the expensive sorting operation only when the data we are running the operation on changes. This is where the concept of memoization comes to the rescue. If a function is called with the same inputs as before, the function can skip doing the actual work, and return the same result it generated the last time it received those input values.

!!! info
    We have been creating our own selectors in the examples above, but there is an excellent library out there called **Reselect** that helps with creating **memoized selectors**.

### Handling action creators, reducers, selectors efficiently

!!! info "Reduxsauce"
    This is the lib that you should use for creating highly readable redux code, and much easier to maintain redux code in your app.
