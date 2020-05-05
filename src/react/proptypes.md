# PropTypes

> Only works on development environment.

## Why

`propTypes` help document your components, which benefits future development in two ways.

1. You can easily open up a component and check which props are required and what type they should be.
2. In development env, when things get messed up, React will give you an error message in the console, saying which props are wrong/missing and the render method that caused the problem.

## How it works

```js
static propTypes = {
  num: PropTypes.number.isRequired
  str: PropTypes.string
}
```

| Right to Left checking                                                                                                                                  | defaultProps                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| In case of `num`, `isRequired` is present. So first it check if value is `undefined` or `null`. If not then only it check if `num` is of type `number`. | `defaultProps` runs before `propTypes`, so type checking also works on default values. |
| `null` and `undefind` are allowed if `isRequired` is not present. | If prop is missing or has a value `undefined` then default value does not change.
| - | If prop has value `null` then default value is replaced with `null`

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

## How to handle props

!!! important
    1. `PropTypes` is just for comp documentation and to fascilitate development process.
    2. **Write the comp by keeping the production env in mind.**

### Accepted types _[whitelisting]_

Whitelist instead of Blacklist. For example, through `propTypes` object, you know that `name` prop is of type `string`. So when you perform check then:

```js
// Recommended.
if(isString(name)) {
  return name.toUpperCase()
}

// Not recommended.
// Condition failed if value either null
// or undefined. So works as expected.
// But name could be: object, array or number, then
// it causes a WSOD.
if(name) {
  return name.toUpperCase()
}
```

!!! note
    Say, you have a prop `isSomthing` which is of type `bool` and it can be `null` too. Because you are using `null` value of prop in your comp to perform certain task. In this case, you will have three possible values `true`, `false` and `null`. Have that in mind, and perform check by using these 3 values only.

### Know which prop can cause WSOD or break the comp

- **Always** check prop of type `object` and `array` before use. Because, more likely you will use their built-in methods. If type is different then it will lead to WSOD.

```js
if(Array.isArray(items) && items.length > 0) {

}
```

- **Primitive** type can be ignored if not using their built-in methods. For example: if you have a `name` prop of type `string` and you are just showing the value as it is, then it does not matter if it is `null`, `undefined` or `object`. React ignores the `null` and `undefined` and coerce the other types to string as in case of `object` it will show `[object][Object]`. But it will not cause WSOD.

If you are doing:

```js
<Text>{name.toUpperCase()}</Text>
```

then it could cause WSOD, if `name` is not of type `string`.

### Treat comp single standalone entity

> Even it is a part of some comps hierarchy.

Do not rely on parent comp to validate props. Do not assume that parent comp will send the right type of data. Each comp has its own responsibility to check their props types if they have possibility to cause WSOD.

**Remember:** You only need to worry about those props which can cause WSOD as mentioned above.

Also, take care of setting default values using `defaultProps` in own comp.

!!! note "Do the checking"
    1. Even if parent comp check the prop, child has to do again.
    2. Even if you are not going to use the comp anywhere else in your app. Just under some parent comp and at one place only.

### Data source you need to watch

Data coming from **API.**

#### Handling API data

A few options:

1. You can validate and check the API data in **transform** function.
2. You can validate the check the API data in a function which handle API request like **saga worker** function.

Validating and checking API data means that **transform the API data to match props type** if there is mismatch of type. Lets say, the `name` property is of type `string` and in API response you get value `null` or `undefined` or other type. Then you can set the default value to the `name` property, say `''` empty string _(primitive type can be `null` or `undefined` and you can avoid setting default value to them because each comp also check the prop if they can cause WSOD)_. But in the case of `object` and `array`, if they are `null` or `undefined` then set their default value to `{}` and `[]` respectively.

!!! note
    Even if you check and validate API data in transform function. Each comp has to do props checking. Do not rely on others. Because, in future, if you replace the module that consume API with third-party API consumer, exclude transform function foe some reason, or you make certain mistake in transform function logic, even then you will be assured that comp will not break due to wrong type.

    There is no performance lag, if data is checked twice. One at the time, in transform function and other by comp itself. Also, comp check only those props which can cause WSOD. Remember, you are not checking millions of data twice which can pose some performance issue.

### How to define props using PropTypes in nested comps

Let's have a purpose of `PropTypes` be clear in mind.

> Documentation and fascilitate development process. So we should choose that way which help us to achieve these two purposes and accept the trade-off like multiple warn messages etc.

Two types of props in comp. And the approach for each type should be:

1. Own props.
    - Do `isRequired`.
    - Set default value, if neccessary.

2. Child props - props which just recieved by comp and simply pass down to child.
    - Do not do `isRequired` check. Let it handle by child comp. If prop is a required prop in child then you do not make it required in parent comp. Just define its type and let child handle the required check.
    - Do not set default value on the behalf of child comp. Let it set by child comp.

!!! note
    1. Comp should worry about and define only its own props and child props that comp is passing down to child comp.
    2. Contextual props (props drilling): do not define the prop in intermeddiate comps. Define in the parent (index) comp which recieve the contexual prop and then define it in the child comp which uses it down in the hierarchy.

#### Benefits

1. If you open any comp file in hierarchy you will see all the props that comp is using. Own props and props that pass down to child. You need not to look into other files in hierarchy to have complete picture of all props that comp is using. It serves the documentation purpose well. That is at one place you get all the information about props.
2. Avoid Typescript linter warning. It complains if it detects the prop which is there in the comp just to pass down to child but not defined.
3. Since we are handling `isRequired` check for own props. React will not warn multiple times. Just one warn message from comp which own the prop.

#### Drawback

1. Multiple warnings on mismatch type. So basically, if some comp is down in the hierarchy has type mismatch then all parents from where the prop is being passed down will also complain about the mismatch type. So you will see multiple warm messages on your screen. This is a trade-off you get when you want to achive the documentation benefit.
2. Props boilerplate.

!!! note "On multiple mismatch warnings"
    We can make use of the multiple mismatch warnings drawback in a situation where we want to know the hierarchical path of prop from top to down. We can track the prop path and know all the comps from where the prop is being passed down. All we need to do is to change the prop definition in root comp (index) and you will see the multiple mismatch type warnings from all the comps in the hierarchy from where the prop is being passed down.

!!! info "End Note"
    PropTypes does not work in production - so you have to worry about checking for props that can cause WSOD. And PropTypes is there for documenting comp and make a developer life easier.
