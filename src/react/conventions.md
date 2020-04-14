# Conventions

## General

- Use descriptive variables that indicate the type or use of the variable. Prefer longer more descriptive names rather than short names.

```js
// Bad
let x, x2, y, z, xl, myvar;

// Good
let counter, systemPreferences, checkoutScreen, addToCartButton;
```

- Do not hardcode values.

```js
// Bad
if (name == “Harvey”)
if (height == 25)

// Good
const DEFAULT_MAX_WIDTH = 25;
const DEFAULT_NAME = 'Person';

if (name == DEFAULT_NAME)
if (height == DEFAULT_MAX_WIDTH)
```

## React

- File name is same as component name. Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name.
- Component name in PascalCase, because react components and HTML tag are differentiate using this.
- props and state must be in camelCase.
- Higher-order component naming

```js
// Bad
export default function WithFoo(WrappedComponent) {
  return function withFoo(props) {
    return <WrappedComponent {...props} foo />;
  }
}

// Good
// camelCase
export default function withFoo(WrappedComponent) {
  // PascalCase
  return function WithFoo(props) {
    return <WrappedComponent {...props} foo />;
  }
}
```

- camelCase for component instances.

```js
// Bad
const ReservationItem = <ReservationCard />;

// Good
const reservationItem = <ReservationCard />;
```

- Use functional component if you are not using state or lifecycle methods.
- Functional component - use function declaration rather function expression and arrow function. Because anonymous and unamed function expression are difficult to debug. **Relying on function name inference is discouraged.**

```js
//Bad
const Listing = ({ hello }) => (
  <div>{hello}</div>
);

// Good
function Listing({ hello }) {
  return <div>{hello}</div>;
}
```

- Always use double quotes (") for JSX attributes, but single quotes (') for all other JS.

```js
// Bad
<Foo bar='bar' />
// Good
<Foo bar="bar" />

// Bad
<Foo style={{ left: "20px" }} />
// Good
<Foo style={{ left: '20px' }} />
```

- Do not use underscore prefix for internal methods of a React component.

```js
// Bad
React.createClass({
  _onClickSubmit() {
    // do stuff
  },
});

// Good
React.createClass({
  onClickSubmit() {
    // do stuff
  },
});
```

- Event naming

    1. Event's prop name prefix with `on`.
    2. Event handler function name: replace `on` with `handle`.

```js
<Comp
  onAddStore={this.handlerAddStore}
/>
```

- Ordering

```
static methods
constructor
getDerivedStateFromProps
componentDidMount
shouldComponentUpdate
componentDidUpdate
componentWillUnmount
clickHandlers or eventHandlers like onClickSubmit() or onChangeDescription()
getter methods for render like getSelectReason() or getFooterContent()
optional render methods like renderNavigation() or renderProfilePicture()
render
```
