# How to handle props

!!! quote
    In programming, you cannot achieve all. In order to achive something you have to accept the trade-off that comes with it.

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

## Know which prop can cause WSOD or break the comp

- **Always** check prop of type `object` and `array` before use. Because, more likely you will use their built-in methods. If type is different then it will lead to WSOD.

```js
if(Array.isArray(items) && items.length > 0) {

}
```

- **Primitive** type can be ignored if not using their built-in methods. For example: if you have a `name` prop of type `string` and you are just showing the value as it is, then it does not matter if it is `null`, `undefined` or any. React ignores the `null` and `undefined` and coerce the other types to string as in case of `object` it will show `[object][Object]`. But it will not cause WSOD.

If you are doing:

```js
<Text>{name.toUpperCase()}</Text>
```

then it could cause WSOD, if `name` is not of type `string`.

## Treat comp single standalone entity

Do not rely on parent comp to validate props and do not assume that parent comp will send the right type of data. **Each comp has its own responsibility to check their props types if they have possibility to cause WSOD.**

!!! note "defaultProps"
    Also, take care of setting default values using `defaultProps` in own comp.

## Multiple checking

- Even if transform function validate and send right data to redux store, the selector function has to do again.
- Even if selector function validate and send right data to comp, the comp has to do again.
- Even if parent comp check the prop, child has to do again.

!!! question "Multiple checking - isn't too much checking?"
    API->Selector->Container->Component

    More or less there are 4 times the prop can be checked. But it is not done to all props, especially, prop which has type `object`, `array` and primitive type which use in-built function in comp and if has different type then can cause WSOD.

    - Say, 4 diffrerent developers are working on API, Redux selector, Container, and Component. So you cannot not rely that other developer do the validation, so you do at your end.
    - Checking a prop multiple times in different steps does not hit performance. **It sounds boilerplate** but you can accept this trade-off to achieve error free application to certain level. _For example, you or other developer when refactoring the code, miss the validation or does the wrong validation in selector or container then child comp should be able to handle the wrong data and does not crash._

## Data source you need to watch

Data coming from **API.**

A few options:

- You can validate API data in **transform** function and send the right type of data to comp.
- If you are not using transform function then you can do the validation process in **selectors** function.
