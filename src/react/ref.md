# ref and ref and forwardRef

## ref

!!! info "Equivalent to"
    - HTML element => `document.getElementById`
    - Component => Instance of component _[new MyComp()]_

`props` is the reccomended way in React to interact with child comp. You do it by sending the updated `props` to child. But in some cases, you do not to take a help of `props` and access the comp directly and perform certain operation. This is where `ref` comes in to picture.

### Use cases

- Managing focus, text selection, or media playback.
- Triggering animations.
- Open/Close modal without `prop`. For example, instead of `isOpen` prop you expose `open()` and `close()` methods on a Modal component.

!!! note "Don’t Overuse ref"
    The stuff that can be done using `props` can be done by `props` and do not use it unneccesary.

### Reserved keyword

`ref` is a reserved keyword for prop. You cannot use `ref` as prop name for other data type. Type of data has to be node reference that either HTML element or component.

If you want to pass down `ref` to child then use another name say `inputRef={textInputRef}` and then access it in child comp using `props.textInputRef`.

!!! important "HOC and ref"
    `ref` will not get passed through to HOC. That’s because `ref` is not a prop. Like `key`, it’s handled differently by React.

    By using `forwardRef` you can pass `ref` to HOC.

### How it works

#### Types of node and value of ref

1. **HTML element:** `ref` = DOM element.
2. **Class Component:** `ref` = instance of the component.

!!! info "What about Function component"
    We cannot use `ref` in function component because function comp does not have instance. Like for class comp, React can create the instance of comp using `new` and then assign the instance to `ref`. This cannot be done in function comp.

#### Example of ref in function comp

By default, you cannot not use the `ref` on function components because they don’t have instances. For example:

```js
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will *not* work!
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```

`MyFunctionComponent` is a function comp and `ref` does not work here.

##### Solution

1. Use `forwardRef`.
2. Convert the comp into class component.

#### Can I use ref inside function comp?

You can use `ref` inside the function comp as long as you refer to a **HTML element** or a **Class component**.

```js
function CustomTextInput(props) {
  // textInput must be declared here so the ref can refer to it
  const textInput = useRef(null);

  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

### Callback ref

> Another way to create `ref`

Instead of passing a `ref` attribute created by `createRef()`, you pass a function. The function receives the component instance or HTML element as its argument, which can be stored and accessed elsewhere.

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // Focus the text input using the raw DOM API
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // autofocus the input on mount
    this.focusTextInput();
  }

  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in an instance field (for example, this.textInput).
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

## forwardRef

It is a technique for automatically passing a `ref` through a component to one of its children.

### Two situations that forwardRef handles

#### [1] use ref on function component

By default, you cannot not use the `ref` on function components because they don’t have instances. For example:

```js
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will *not* work!
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```

Now, you can rewrite above example using `forwardRef` to use `ref` on function component.

```js
const MyFunctionComponent = React.forwardRef((props, ref) => ({
  return <input ref={ref} />;
})

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will work!
    return (
      <MyFunctionComponent ref={this.textInput} />
    );
  }
}
```

#### [2] Access the child comp in parent comp

##### Without forwardRef

> This pattern works both for classes and for functional components.

```js
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <CustomTextInput inputRef={this.inputElement} />
    );
  }
}
```

> Remember: `ref` is a reserved keyword for prop name. When React see the `ref` prop, it assign the HTML element or component instance to it automatically.

In the example above,  `Parent`  passes its class property  `this.inputElement`  as an  `inputRef`  prop to the  `CustomTextInput`, and the  `CustomTextInput`  passes the same ref as a special  `ref`  attribute to the  `<input>`. As a result,  `this.inputElement.current`  in  `Parent`  will be set to the DOM node corresponding to the  `<input>`  element in the  `CustomTextInput`.

Note that the name of the  `inputRef`  prop in the above example has no special meaning, as it is a regular component prop. However, using the  `ref`  attribute on the  `<input>`  itself is important, as it tells React to attach a ref to its DOM node.

Another benefit of this pattern is that it works several components deep. For example, imagine  `Parent`  didn't need that DOM node, but a component that rendered  `Parent`  (let's call it  `Grandparent`) needed access to it. Then we could let the  `Grandparent`  specify the  `inputRef`  prop to the  `Parent`, and let  `Parent`  "forward" it to the  `CustomTextInput`:

```js
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

function Parent(props) {
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}

class Grandparent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <Parent inputRef={this.inputElement} />
    );
  }
}
```

Here, the ref `this.inputElement` is first specified by `Grandparent`. It is passed to the `Parent` as a regular prop called `inputRef`, and the `Parent` passes it to the `CustomTextInput` as a prop too. Finally, the `CustomTextInput` reads the `inputRef` prop and attaches the passed ref as a `ref` attribute to the `<input>`. As a result, `this.inputElement.current` in `Grandparent` will be set to the DOM node corresponding to the `<input>` element in the `CustomTextInput`.

##### With forwardRef

```js
const Parent = React.forwardRef((props, ref) => ({
  return (
    <div>
      <input ref={ref} />
    </div>
  );
})

const Parent = React.forwardRef((props, ref) => ({
  return (
    <div>
      My input: <CustomTextInput ref={ref} />
    </div>
  );
})

class Grandparent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <Parent ref={this.inputElement} />
    );
  }
}
```
