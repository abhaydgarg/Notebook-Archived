# Hook rules

## Hooks only works inside Function component.

Its does not work inside Class comp and normal Javascript function.

## Always call hooks at the top level and preserve the order of hooks

Don’t call Hooks inside loops, conditions, or nested functions. Instead, always use Hooks at the top level of your React function. By following this rule, you ensure that Hooks are called in the same order each time a component renders.

### Explanation

> Hooks defined in the function component are handled and managed using an array.

!!! note
      array is taken as an analogy to understand the concept. This is not necessarily how the React works internally.

```js
function RenderFunctionComponent() {
  const [firstName, setFirstName] = useState("Rudi");
  const [lastName, setLastName] = useState("Yardley");
}
```

#### [1] Initialisation

Create two empty arrays: `setters` and `state`

Set a cursor to 0

![1](./assets/1_LAZDuAEm7nbcx0vWVKJJ2w.png)

#### [2] First render

Run the component function for the first time.

Each  `useState()`  call, when first run, pushes a setter function (bound to a cursor position) onto the  `setters`  array and then pushes some state on to the  `state`  array.

![2](./assets/1_8TpWnrL-Jqh7PymLWKXbWg.png)

#### [3] Subsequent render

Each subsequent render the cursor is reset and those values are just read from each array

![3](./assets/1_qtwvPWj-K3PkLQ6SzE2u8w.png)

#### [4] Event handling

Each setter has a reference to its cursor position so by triggering the call to any `setter` it will change the state value at that position in the state array.

![4](./assets/1_3L8YJnn5eV5ev1FuN6rKSQ.png)

### Lets change the order

Now what happens if we change the order of the hooks for a render cycle.

```js
let firstRender = true;

function RenderFunctionComponent() {
  let initName;

  if(firstRender){
    [initName] = useState("Rudi");
    firstRender = false;
  }
  const [firstName, setFirstName] = useState(initName);
  const [lastName, setLastName] = useState("Yardley");
}
```

#### [1] First render

![1](./assets/1_C4IA_Y7v6eoptZTBspRszQ.png)

At this point our instance vars `firstName` and `lastName` contain the correct data but let’s have a look what happens on the second render:

#### [2] Second render

![2](./assets/1_aK7jIm6oOeHJqgWnNXt8Ig.png)

Now both `firstName` and `lastName` are set to `“Rudi”` as our state storage becomes inconsistent.
