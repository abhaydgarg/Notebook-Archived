# Next features

## Nullish coalescing operator

> ECMA11 – 2020

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

## Logical assignment operators

> ECMA12 – 2021

They are syntatic sugar similar to `x += 1` => `x = x + 1`.

| Operator  | Equivalent to | Only assigns if `a` is |   |   |
|-----------|---------------|------------------------|---|---|
| a \|\|= b | a \|\| (a=b)  | Falsy                  |   |   |
| a &&= b    | a && (a=b)    | Truthy                 |   |   |
| a ??= b    | a ?? (a=b)    | Nullish                |   |   |
