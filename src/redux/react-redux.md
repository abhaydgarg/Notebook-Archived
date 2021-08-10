# react-redux

## Handling action creators, reducers, selectors efficiently

!!! info "redux-toolkit or reduxsauce"
    These are the libraries that you should use for creating highly readable redux code, and much easier to maintain redux code in your app.

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
