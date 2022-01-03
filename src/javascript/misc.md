# Misc
## Empty string => Boolean

The result is `false` if empty String (its length is zero); otherwise the result is `true`.

```js
const str = ''
console.log(str || 1) // 1
```

## void

Evaluates the given expression and then returns `undefined`.

This can be used in arrow function to avoid **Non-leaking.**

```js
// Without void the result of doSomething()
// function is returned. Using void, we are
// saying, run doSomething() and return
// undefined.
const onclick = () => void doSomething();
```
