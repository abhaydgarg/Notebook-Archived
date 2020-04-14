# PropTypes

## How to validate and check props

It is the responsibility of parent component to send the data down to children using props on the basis of the child's `Proptypes` rules. Let the parent validate (checking data type and value) the props of children and send appropraite props as required by children. And leave the children out from explicit validation process. In some cases, child should check the prop's value before render in order to avoid unexpected error.

**Strategy:** Data transformer function or parent comp is the place to validate props and set default value of children props as expected by children comp. So that right data type is send to component. Care is taken for the props which **get the value from API response.**

!!! note "Points to keep in mind"
    1. `array`, `object` and `function` are the props that cause white screen of death. Even when checked using `PropTypes` can cause WSOD. So if you are getting `props` such as `array`, `object` from API then you should check them again such as their type and emptiness when using in comp. As far as `function` is concern you can see if checking of `function` is required or not, because if you declare a function in parent and pass it to the child comp then you can mark function required in child comp but there is no need to wrap it in `if` statement to check if it is `function` or not because it will be there always and there is no way it will be missing.
    2. For primitive types there is no need to check because `null` and `undefined` will not be shown and ignored. Explicit type and empty check is required if say, you have a `string` and you want to apply string's built-in method. If you are just displaying then do not worry much. You can set default value though in case if `prop` is missing or `undefined`.
    3. You cannot set a hard and fast rule for how to use `PropTypes` and which `prop` should be checked explicitly in comp before using to avoid WSOD. Figure out suitable approach for the `prop` based on how it is going to be used and where it is coming from (API or not).

## propTypes

`propTypes` help document your components, which benefits future development in two ways.

1. You can easily open up a component and check which props are required and what type they should be.
2. When things get messed up, React will give you an error message in the console, saying which props are wrong/missing and the render method that caused the problem.

```js
import React, { Component, PropTypes } from 'react';
import { render } from 'react-dom';

class Greeter extends Component {
  render() {
    return (<h1>{this.props.salutation}</h1>)
  }
}

// validation
Greeter.propTypes = {
  salutation: PropTypes.string.isRequired
}
```

## How type checking works in propTypes

```js
static propTypes = {
  num: PropTypes.number.isRequired
  str: PropTypes.string
}
```

Checking done from **Right to Left**.

For example in case of `num`, if `required` is present then `undefined` and `null` are not allowed and warning is generated before checking type `number` of `num`. If not `undefined` and `null` then prop types check type `number`.

If `required` is not supplied then for example in case of `str` then prop types check type `string` **but** `null` and `undefined` are also allowed.

**defaultProps** runs first before **propTypes** and in this way proptypes also works when value get from defaultProps. If default value is present then props receive the default value and then `isRequired` is checked. **Next**, if prop's recieved `undefined` value say from API or parent comp then default value is not replaced with `undefined` but if prop's recieve `null` value then default value is replaced with `null`. _`undefined` then default value is not replaced and `null` then default value is replaced with `null`_.

## Centralize propTypes

In real apps with large objects, using PropTypes quickly leads to a lot of code. That’s a problem, because in React, you’ll often pass the same object to multiple components. Repeating these details in multiple component files breaks the DRY principle (don’t repeat yourself). Repeating yourself creates a maintenance problem. **The solution? Centralize your PropTypes.**

Using the `shape` function of the `prop-types` package it’s possible to define types yourself.

```js
//# userTypes.js
import { shape, number, string, oneOf } from 'prop-types';

export const addressType = shape({
    id: number.isRequired,
    street: string.isRequired,
    street2: string,
    city: string.isRequired,
    state: string.isRequired,
    postal: number.isRequired,
});

export const userType = shape({
  id: number,
  firstName: string.isRequired,
  lastName: string.isRequired,
  company: string,
  role: oneOf(['user', 'author']),
  address: addressType.isRequired,
});
```

and then imports them when needed:

```js
import {userType} from './userTypes';

export default class User extends Component {
  static propTypes = {
    user: userType.isRequired,
  };
}
```

!!! note "Scenario - parent(index) and children comps"
    - Mention all props in propTypes in index file (children's props too). This way, later when you visit comp, you can check all props used by comp by just looking at index file propTypes object.
    - `isRequired` and `defaultProps` should be used in comps which actually use the prop and not just passing it to the child comp. For example, you can mention prop in index file but `isRequired` and `defaultProps` should be used in the child comp which actually use it. Because if you mention `isRequired` in index file too then you will get multiple warning one from index file propTypes and another from child propTypes comp.
