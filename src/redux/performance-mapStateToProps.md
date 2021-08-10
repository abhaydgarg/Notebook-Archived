# Performance of mapStateToProps

## It should be fast

!!! important "Why"
    Whenever the store changes, all of the `mapStateToProps` functions of all of the connected components will run. This is very important to understand. This means, say, if you have 10 comps connected (subscribed) to store by `mapStateToProps` and store's state get changed. Now react-redux does not know which comp is acually using the property that has changed. So react-redux runs `mapStateToProps` function of all 10 comps and compare the old and new values of `mapStateToProps` if it sees the change in any of the value then it re-render the comp or otherwise skip the render. Because of this, your `mapStateToProps` function should run as fast as possible.

### How react-redux ensures the performance of mapStateToProps

`connect` is a HOC comp, which uses the `shouldComponentUpdate` method internally and when `mapStateToProps` function is executted. React Redux decides whether the contents of the object returned from `mapStateToProps` are different from previous using `===` comparison (a "shallow equality" check) on each fields of the returned object. If any of the fields have changed, then your component will be re-rendered so it can receive the updated values as props.

> This way react-redux avoid the reconciliation process _(skip running render method, so no need to compare old virtual DOM with new one and mutate the DOM if found any change)_ which is a huge benefit for performance.

### Developer duty

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
