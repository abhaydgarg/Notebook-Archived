# Components

```js
// ## 1 Class - with lifecycle methods.
import React from 'react';
class Hello extends React.Component {
  render() {
    return (
      <h1> Hello World </h1>
    )
  }
}

// ## 2 Function way without any state lifecycle methods.
// stateless component just return jsx based
// on props (recommnded for stateless component)

// note: this way does not use render() method whatever retrun from
// the function is treated as if returned from render() method.
const MyComponent = props => {
  return (
    <div className={props.className}/>
  );
};

 // OR

const MyComponent = function(props) {
  return (
    <div className={props.className}/>
  );
};
```

## Dynamic values

Values written between curly braces `{}` are evaluated as a JavaScript expression and rendered in the markup.

```js
class Hello extends Component {
  render() {
    var place = 'World';
    return (
      <h1>Hello {place}</h1>); // using dynamic value
  }
}
```

## Props

A key factor to make components reusable and composable is the ability to configure them, and React provides properties (or props, in short) for doing so.

Props are of key importance in component composition.

> 1. They are the mechanism used in React for passing data from parent to child components.
> 2. Props can’t be changed from inside the component (immutable); they are passed and “owned” by the parent.

### super(props)

**When you want to access **`this.props`** in constructor.**

```js
// ## Passing
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    console.log(this.props) // { icon: 'home', … }
  }
}

// ## Not passing
// passing or not passing props to super has no effect on later uses of this.props outside
// constructor. That is render, shouldComponentUpdate, or event handlers always have access
// to it using this.props

class MyComponent extends React.Component {
  constructor(props) {
    super()
    console.log(this.props) // undefined
    // Props parameter is still available
    console.log(props) // { icon: 'home', … }
  }
  render() {
    // No difference outside constructor
    console.log(this.props) // { icon: 'home', … }
  }
}
```

## State

`Props` are immutable, so in order to have mutable data react has `state` concept.

React’s components can have mutable data inside `this.state`.

**Note** that `this.state` is private to the component and can be changed by calling `this.setState()`.

### How to re-render component and its children

`this.setState()`

When the state is updated using `this.setState()`, react triggers the re-rendering, and the component itself and its children will be re-rendered. This happens very quickly due to React’s use of a virtual DOM.

## Component types

* **Stateless Component** — Only _props_, no _state._ There's not much going on besides the `render()` function and all their logic revolves around the _props_ they receive. They recieve data from parent by props. We sometimes call these _**Dumb Component**_.
* **Stateful Component** — Both _props_ and _state._ We also call these _state managers_. They are in charge of client-server communication (XHR, web sockets, etc.), processing data and responding to user events.

> Dumb components may trigger logic, like updating state, but only by means of functions passed down from the parent component.

## props vs state

| _props_ | _state_ |
| --- | --- |
| immutable | mutable |
| used to pass data from parent to child component | state of parent can be passed to child component using props. **Do not access state of parent directly in child.** |

