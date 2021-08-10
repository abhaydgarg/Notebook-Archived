# Passing function

## Why is binding necessary at all?

In JavaScript, these two code snippets are not equivalent:

```js
obj.method(); // Context: obj
```

```js
var method = obj.method;
method(); // Context: global
```

In React, when we pass the function to child comp using props, for example:

```js
<Child
  onClick={this.handleClick}
>
```

This simply means as mentioned above.

```js
props.onClick = this.handleClick;
props.onClick() // Context is changed to child.
```

## Why is my function being called every time the component renders?

Make sure you aren’t calling the function when you pass it to the component:

```js
render() {
  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>Click Me</button>
}

// Pass the reference.
render() {
  // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>Click Me</button>
}
```

## How do I pass a parameter to an event handler or callback?

You can use an arrow function to wrap around an event handler and pass parameters:

```js
<button onClick={() => this.handleClick(id)} />

// OR
<button onClick={this.handleClick.bind(this, id)} />
```

!!! note "Caveat"
    Using an arrow function or bind as above in render creates a new function each time the component renders, which may break optimizations for performance because when React compares the identity of the comp on the basis of, whether its props has changed then it will see the change in the props everytime and render the comp on DOM.

### How to avoid the caveat?

#### Passing params using data-attributes

Alternately, you can use DOM APIs to store data needed for event handlers.

```js
const A = 65 // ASCII character code

class Alphabet extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
    this.state = {
      justClicked: null,
      letters: Array.from({length: 26}, (_, i) => String.fromCharCode(A + i))
    };
  }

  handleClick(e) {
    this.setState({
      justClicked: e.target.dataset.letter
    });
  }

  render() {
    return (
      <div>
        Just clicked: {this.state.justClicked}
        <ul>
          {this.state.letters.map(letter =>
            <li key={letter} data-letter={letter} onClick={this.handleClick}>
              {letter}
            </li>
          )}
        </ul>
      </div>
    )
  }
}
```

!!! note "React Native"
    RN does not have a concept of setting data in data-attributes and then access them by using `e.target.dataset.letter` as mentioned above. So in RN, you have to do:

    `onPress={() => this.handleClick(id)}`

##### Alternative in RN

Create a child comp, pass the function reference and id as props.

```js
//# Parent.
class Parent extends Component {
  onPress = (id) => {
    console.log(id)
  };

  render() {
    return (
      <div>
        <Child
          id={123}
          onPress={this.onPress}
        />
      </div>
    );
  }
}

// Child.
class Child extends Component {
  onPress = (id) => {
    this.props.onPress(this.props.id)
  };

  render() {
    return <Button onClick={this.onPress}>Click</Button>
  }
}
```

!!! question "Is it OK to use arrow functions in render methods?"
    Generally speaking, yes, it is OK, and it is often the easiest way to pass parameters to callback functions.

    > If you do have performance issues, by all means, optimize!
