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

## Optional Chaining

The optional chaining operator provides a way to simplify accessing values through connected objects when it's possible that a reference or function may be `undefined` or `null`.

```javascript
// Earlier.
const stop = please && please.make && please.make.it && please.make.it.stop;
// or use a library like 'object-path'.
const stop = objectPath.get(please, "make.it.stop");

// With optional chaining.
const stop = please?.make?.it?.stop;
```

By using the `?.` operator instead of just `.`, JavaScript knows to implicitly check to be sure `obj.first` is not `null` or `undefined` before attempting to access `obj.first.second`. If `obj.first` is `null` or `undefined`, the expression automatically short-circuits, returning `undefined`.

### Optional chaining with function calls

Using optional chaining with function calls causes the expression to automatically return undefined instead of throwing an exception if the method isn't found:

```javascript
let result = someInterface.customMethod?.();
```

!!! note
    **Look:** `?.` not just `?`

    If there is a property with such a name and which is not a function, using `?.` will still raise a TypeError exception (`x.y` is not a function).

### Optional chaining with array

```javascript
arr?.[2];
```

!!! note
    **Look:** `?.` not just `?`

## Nullish coalescing operator

The nullish coalescing operator returns the results of the right hand-side expression if the left hand-side expression is either `null` or `undefined`.

However, due to `||` being a boolean logical operator, the left hand-side operand was coerced to a boolean for the evaluation and any _falsy_ value (`0`, `''`, `NaN`, `null`, `undefined`) was not returned. This behavior may cause unexpected consequences if you consider `0`, `''`, or `NaN` as valid values.

```javascript
let count = 0;
let text = "";

let qty = count || 42;
let message = text || "hi!";
console.log(qty);     // 42 and not 0
console.log(message); // "hi!" and not ""
```

The nullish coalescing operator avoids this pitfall by only returning the second operand when the first one evaluates to either `null` or `undefined` (but no other falsy values):

```javascript
let myText = ''; // An empty string (which is also a falsy value)
let preservingFalsy = myText ?? 'Hi neighborhood';
console.log(preservingFalsy); // '' (as myText is neither undefined nor null)
```
