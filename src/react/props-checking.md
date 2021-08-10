# Handling props

!!! info "Trade-off"
    You cannot achieve all. There is always a trade-off when you choose something over another. Know the trade-off and what you are trying to achieve.

!!! note "Component = Single standalone unit"
    Do not rely on parent comp to validate props and do not assume that parent comp will send the right type of data. **Each comp has its own responsibility to check their props type if they have possibility to cause WSOD.**

    Also, set default values using `defaultProps` in own comp.

## Whitelist

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

## Know which prop can cause WSOD

- **Always** check prop of type `object` and `array` before use. Because, more likely you will use their built-in methods. If type is different then it will lead to WSOD.

```js
if(Array.isArray(items) && items.length > 0) {

}
```

- **Primitive** type can be ignored if not using their built-in methods. For example, if you have a `name` prop of type `string` and you are just showing the value then it does not matter if it is `null`, `undefined` or any. React ignores the `null` and `undefined` and coerce the other types to string as in case of `object` it will show `[object][Object]`. But it will not cause WSOD.

But if you are using built-in method then it could cause WSOD, if `name` is not of type `string`.

```js
<Text>{name.toUpperCase()}</Text>
```

## Multiple checking

- Even if transform function validate and send right data to redux store, the selector function has to do again.
- Even if selector function validate and send right data to container, the container has to do again.
- Even if parent comp check the prop, child has to do again.

### isn't too much checking?

> API -> Selector -> Container -> Component

Following are some benefits that you can have while accepting the multiple checking as a trade-off.

- **Reusability:** When selector, container and component are reused then we do not worry about validation. If you do the validation in a container and not in component then while reusing the comp, you have to move the validation code from container to component.
- **Less error prone:** Multiple checking may sound **boilerplate** but it makes you app less prone to an error when one part or another is changed or refactored later. _For example, you or other developer when refactoring the code, miss the validation or does the wrong validation in selector or container then child comp should be able to handle the wrong data and does not crash the app._
- **Diffrerent developers** can work on API, Redux Selector, Container, and Component. And you need not to rely on other developer if he has done the validation.

#### Performance

Checking a prop multiple times in every step does not hit performance because you are just checking the `type` and nothing more. And you are doing the checking for those props which can cause WSOD and not all.

## Data source you need to watch

Data coming from **API.**

A few options:

- You can validate API data in **transform** function and send the right type of data to comp.
- If you are not using transform function then you can do the validation process in **selectors** function.
