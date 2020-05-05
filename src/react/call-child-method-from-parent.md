# Call child method from parent

## Pattern [1] - ref

### Using class component

```js
class Parent extends Component {
  constructor(props) {
    super(props);
    this.child = React.createRef();
  }

  onClick = () => {
    this.child.current.getAlert();
  };

  render() {
    return (
      <div>
        <Child ref={this.child} />
        <button onClick={this.onClick}>Click</button>
      </div>
    );
  }
}

class Child extends Component {
  getAlert() {
    alert('getAlert from Child');
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

### Using hooks and function component

> Previously, `ref` were only supported for Class-based components. But using `forwardRef` you can use it function component too.

```js
const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    getAlert() {
      alert('from Child');
    }
  }));

  return <h1>Hi</h1>;
});

const Parent = () => {
  // In order to gain access to the child component instance,
  // you need to assign it to a `ref`, so we call `useRef()` to get one.
  const childRef = useRef();

  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

!!! note "Argument"
    Not a good practice: This is generally not the way to go about things in React land. Usually what you want to do is pass down functionality to children in props. It's more of an escape hatch when you need it, and should be used only in emergencies.

    _If best practice is to create a maze of logic to do something as simple as calling a child component's method - then I disagree with best practice._

## Pattern [2]

```js
class Parent extends Component {
 render() {
  return (
    <div>
      <Child setClick={click => this.clickChild = click}/>
      <button onClick={() => this.clickChild()}>Click</button>
    </div>
  );
 }
}

// Note you can't use `onClick={this.clickChild}` in parent
// because when parent is rendered child is not mounted so
// `this.clickChild` is not assigned yet. Using
// `onClick={() => this.clickChild()}` is fine because
// when you click the button `this.clickChild` should
// already be assigned.

class Child extends Component {
 constructor(props) {
    super(props);
    this.getAlert = this.getAlert.bind(this);
 }

 componentDidMount() {
    this.props.setClick(this.getAlert);
 }

 getAlert() {
    alert('clicked');
 }

 render() {
  return (
    <h1 ref="hello">Hello</h1>
  );
 }

}
```
