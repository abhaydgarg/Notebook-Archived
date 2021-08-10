# setState(updater [, callback])

## Two ways to use it

### [1] Without updater function

```js
this.setState({
  value: 1
});
```

### [2] Using updater function

`props` parameter - current props.

```js
this.setState((prevState, props) => {
  return {
    value: prevState.value + 1
  };
});
```

### callback argument

`callback` function is called when state is updated completely using `setState`. As you know `setState` update state asynchronously so haivng callback to perfom certain action after the state is updated is useful.

```js
this.setState((prevState, props) => {
  return {
    value: prevState.value + 1
  };
}, () => {
  // Updated state.
  console.log(this.state);
});
```

## Update state based on previous state

```js
// ### Wrong - no guarantee that `this.state.value`
// gives you previous state because setState() update this.state
// asynchronously
this.setState({
  value: this.state.value + 1
});

// ### Right - use updater callback, it guarantee that
// you always get previous state of `value`
this.setState((prevState, props) => {
  return {value: prevState.value + 1};
});
```

## `setState` updates the local state asynchronously

You should think of `setState` as a request rather than an immediate command. It is not guaranteed that the state changes are applied instantly after a `setState` call.

### Important

> A common mistake is to read `this.state` right after calling `setState`. Instead, use `componentDidUpdate` or a `setState` second argument callback `setState(updater, callback)`, either of which are guaranteed to fire after the update has been applied.

## Multiple calls are batched together

In React 16 or earlier, there are two situations where react batch multiple calls. To be clear, this only works in React-controlled synthetic event handlers and lifecycle methods. State updates are not batched in AJAX and setTimeout event handlers.

1. React event handlers `<div onClick />`
2. React lifecycle methods `componentDidMount() etc`

> In future versions (probably React 17 and later), React will batch all updates by default.

### What does batch multiple calls means ?

No matter how many `setState()` calls you do inside a React event handler or life-cycle method, they will produce only a single re-render at the end of the event.

The updates are always shallowly merged in the order they occur. For example;

```js
// three calls
this.setState({
  a: 10  // Doesn't re-render
});
this.setState({
  b: 20  // Doesn't re-render
});
this.setState({
  a: 30  // Doesn't re-render
});
```

You will not see intermediate re-render with `{a: 10}` , `{b: 20}` , and `{a:30}`, just one after all calls are merged together.

React batch/merge all of them and produce one call by `setState()` and the final render state would be,

```js
// merge all three call into one and re-render
this.setState({
  a: 30,
  b: 20
});
```

If multiple calls are done outside, react event handler or life cycle methods then, you will see intermediate re-render or in other words multiple re-render.

So if in example below we have an AJAX response handler instead of handleClick, each `setState()` would be processed immediately as it happens.

```js
// We're not in an event handler, so these are flushed separately.
promise.then(() => {
  this.setState({a: true}); // Re-renders with {a: true, b: false }
  this.setState({b: true}); // Re-renders with {a: true, b: true }
});
```
