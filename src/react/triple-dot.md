# Triple dots

React component is actually a function. You can use `...` in React for:

## [1] Passing props to component

```js
// say you have a component `CompOne`
const CompOne = props => {
  return <div>CompOne has props - {props.x}, {props.y}, {props.z}</div>;
};

// And when you use `CompOne` in another component
// then there are two ways to pass props to `CompOne`
function App() {
  // pass props one by one separated by space
  return <CompOne
            x={1}
            y={2}
            z={3}
        />;
}

// OR

function App() {
  // pass props define in object
  // and the use `...`
  let obj = { x: 1, y: 2, z: 3 };
  return <CompOne {...obj} />;
}

//# Caveats
function App() {
  // passing props using `...`, so if
  // you pass prop which already exist in
  // `obj`, for example `z` is passed separately
  // so the value 3 define in object override
  // by 4. Therefore, CompOne's props `z` has value 4.
  let obj = { x: 1, y: 2, z: 3 };
  return <CompOne {...obj} z={4} />;
}

// What if i change the order the way
// props are passed
function App() {
  // the value of z remain 3
  // so order matters.
  let obj = { x: 1, y: 2, z: 3 };
  return <CompOne z={4} {...obj} />;
}
```

## [2] Copying and merging props and state (Does shallow copy)

> `Object.assign()` is used for copying and merging.

```js
// # Copying

let obj1 = { foo: 'bar', x: 42 };
// change clonedObj does not change obj1
let clonedObj = { ...obj1 };

// # Merging
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };
var mergedObj = { ...obj1, ...obj2 };
// same key from obj2 replace the value of obj1
console.log(mergedObj) // { foo: "baz", x: 42, y: 13 }

// OR

var obj1 = { foo: 'bar', x: 42 };
var mergedObj = { ...obj1, foo: 'baz', z: 14 };
console.log(mergedObj) // { foo: "baz", x: 42, y: 13, z: 14}
```

## [3] Destructuring props and state

```js
// say following props are passed
// down from parent
props = { x: 1, y: 2, z: 3};

const CompOne = props => {
  // say x is used by this component `CompOne`
  // and rest of the props are used by `CompTwo`
  // so you can extract `x` and put rest props in
  // `rest` variable.
  let {x, ...rest} = props;
  return (
    <div>
      <div>Component One - {props.x}</div>
      <CompTwo {...rest} />
    </div>
  );
};

const CompTwo = props => {
  return <div>CompTwo has props - {props.y}, {props.z}</div>;
};
```
