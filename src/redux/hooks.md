# Hooks

## useStore()

This hook returns a reference to the same Redux store that was passed in to the `<Provider>` component.

> Prefer `useSelector()` as your primary choice.

```js
import React from 'react'
import { useStore } from 'react-redux'

export const CounterComponent = ({ value }) => {
  const store = useStore()

  // EXAMPLE ONLY! Do not do this in a real app.
  // The component will not automatically update if
  // the store state changes.
  return <div>{store.getState()}</div>
}
```

## useSelector()

### When it runs
1. The selector will be run whenever the function component renders (unless its function reference hasn't changed since a previous render of the component so that a cached result can be returned by the hook without re-running the selector's function). I do not think it is big deal, whether function run or not. All we need to care about the return value that we get because it is what detrmines if component need to be render or not. The second point.
2. Using `useSelector()` will automatically subscribe to the Redux store, and run your selector whenever an action is dispatched. But `useSelector()` only forces a re-render if the selector result appears to be different than the last result. Just like `connect` that all `mapStateToProps` will run if state get changed.

### Difference from mapStateToProps
- The selector may return any value as a result, not just an object.
- The selector function does not receive an `ownProps` argument. However, props can be used through closure or by using a curried selector.
- React Redux v7 uses React update batching behavior. Therefore, `useSelector()` can be called multiple times within a single function component and a dispatched action that causes multiple `useSelector()`s in the same component to return new values should only result in a single re-render.

### Comaprison and update

In `connect`, react does a shallow comparison to object returned by `mapStateToProps`. With mapState, all individual fields were returned in a combined object. It didn't matter if the return object was a new reference or not - `connect()` just compared the individual fields.

Whereas, in `useSelector()` react does reference equality check. Therefore returning a new object every time will always force a re-render by default. In example below, `useSelector` is returning a different object literal each time it’s called. When the store is updated, React Redux will run this selector, and since a new object was returned, always determine that the component needs to re-render, which isn’t what we want.

```js
const { count, user } = useSelector(state => ({
  count: state.counter.count,
  user: state.user,
}));
```

#### Solution

If you want to retrieve multiple values from the store, you can:

##### [1]

Call `useSelector()` multiple times, with each call returning a single field value.

```js
const count = useSelector(state => state.counter.count);
const user = useSelector(state => state.user);
```

##### [2]

Use the shallowEqual function from React-Redux as the equalityFn argument to useSelector(), like:

```js
import { shallowEqual, useSelector } from 'react-redux';

const { count, user } = useSelector(state => ({
  count: state.counter.count,
  user: state.user,
}), shallowEqual);
```

### Performance

By default `useSelector()` will do a reference equality comparison of the selected value when running the selector function after an action is dispatched, and will only cause the component to re-render if the selected value changed.

However, unlike` connect()`, `useSelector()` **does not prevent the component from re-rendering due to its parent re-rendering, even if the component's props did not change.**

If further performance optimizations are necessary, you may consider wrapping your child function component in `React.memo()`:

```js
const CounterComponent = ({ name }) => {
  const counter = useSelector(state => state.counter)
  return (
    <div>
      {name}: {counter}
    </div>
  )
}

export const MemoizedCounterComponent = React.memo(CounterComponent)
```

### How to use props in useSelector

```js
// Curried functions
const getStateById = state => id => state.todo.byId[id];
const getIdByState = id => state => state.todo.byId[id];

const TodoComponent = () => {
  const id = 1;

  // Curried - avoid - force re-render because useSelector get
  // new function reference each time, therefore avoid.
  const todoCurried = useSelector(getStateById)(id);
  // Curried - prefer - do not force render because useSelector get
  // a value and the value is taken into account to decide
  // if force re-render to be run if value is changed.
  // Prefer this way.
  const todoCurried2 = useSelector(getIdByState(id));

  // Closure - prefer because re-render is judged by value retruned.
  const todoClosure = useSelector(state => state.todo.byId[id]);

  // Curried + Closure - prefer because re-render is judged by value retruned.
  const todoNormal = useSelector(state => getStateById(state)(id));
};
```
