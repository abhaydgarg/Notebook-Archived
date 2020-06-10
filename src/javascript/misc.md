# Misc

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
