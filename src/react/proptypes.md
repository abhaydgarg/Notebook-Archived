# PropTypes

> Only works on development environment.

!!! important
    1. `PropTypes` is just for comp documentation and to fascilitate development process.
    2. Write the comp by keeping the **production env in mind.**

## Why

`propTypes` help document your components, which benefits future development in two ways.

1. You can easily open up a component and check which props are required and what type they should be.
2. In development env, when things get messed up, React will give you an error message in the console, saying which props are wrong/missing and the render method that caused the problem.

### Using isRequired

> isRequired => prop cannot be `undefined` or `null`.

Make those prop `isRequired`, if it is missing then comp will break.

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

### How to define props using PropTypes in nested comps

Let's have a purpose of `PropTypes` be clear in mind.

> Documentation and fascilitate development process. So we should choose the way which help us to achieve these two purposes.

Two types of props in comp. And the approach for each type should be:

1. Own props.
    - Do `isRequired`.
    - Set default value, if neccessary.

2. Child props - props which just recieved by comp and simply pass down to child.
    - Do not do `isRequired` check. If prop is a required prop in child then you do not make it required in parent comp. Just define its type and let child handle the required check.
    - Do not set default value on the behalf of child comp. Let it set by child comp.

!!! note
    Contextual props (props drilling): do not define the prop in intermeddiate comps. Define in the parent (index) comp which recieve the contexual prop and then define it in the child comp which uses it down in the hierarchy.

#### Benefits

1. If you open any comp file in hierarchy you will see all the props that comp is using. Own props and props that pass down to child. You need not to look into other files in hierarchy to have complete picture of all props. It serves the documentation purpose well. That is at one place you get all the information about props.
2. Since we are handling `isRequired` check for own props. React will not warn multiple times for `isRequired` failure.

#### Drawbacks

1. Multiple warnings on mismatch type. So basically, if some comp is down in the hierarchy has type mismatch then all parents from where the prop is being passed down will also complain about the mismatch type. So you will see multiple warm messages on your screen. **This is a trade-off you get when you want to achive the documentation benefit.**
2. Props boilerplate.

!!! note "Multiple mismatch warnings"
    We can make use of the multiple mismatch warnings drawback in a situation where we want to know the hierarchical path of prop from top to down. We can track the prop path and know all the comps from where the prop is being passed down. All we need to do is to change the prop type in root comp (index) and you will see the multiple mismatch type warnings from all the comps in the hierarchy from where the prop is being passed down.

## Can I not use PropTypes?

Yes, it is not neccessary to use PropTypes. The only thing that you miss is comp documentation and developer may have hard time figuring out the props that comp is using and their types.

If you do the check for props that can cause WSOD then we do not need it. Because this is what matters in production.

## PropTypes vs Typescript

PropTypes help to check props at the runtime when you have data coming from API or other source and there is a possibility that data from API can have different types as compare to props type in comp.

Typescript does not do the runtime check. It helps the developer by checking the props at compile time.

### Which one to use

Again - the purpose - documentation and early warning system for developer.

**If you are not using Typescript** in your app then you should use PropTypes. So you, when later visit your comp and other developer when look into your comp then you and other developer can know about comp props and their types quickly and without a hasssle.

**If you are using Typescript** in your app then do not use PropTypes. Use Typescript instead, because in Typescript you create a type definition file for your comp which tells you and other developer about the props and their types. It serves the purpose of documentation and warn developer about mismatch at compile time too.

!!! question "What about runtime check (API data) when we use Typescript"
    1. You do type checking for those props at the comp level which can cause WSOD or other errors. So there is no need to have another layer of PropTypes to check the types.
    2. You can create a transform function (Adapter) which check and transform the data to match your props type.
