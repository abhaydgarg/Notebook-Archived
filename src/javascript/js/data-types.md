# Data types

## Javascript is dynamic type

This means that the same variable can be used to hold different data types.

```js
var x;  // Now x is undefined
x = 5;   // Now x is a number
x = "John"; // Now x is a string
```

## Primitive types

> A primitive data value is a single simple data value with no additional properties and methods.

There are six types of primitive data types in javascript.

1. number  - `typeof` is `number`
2. boolean - `typeof` is `boolean`
3. string - `typeof` is `string`
4. null - `typeof` is `object` **Note**
5. undefined - `typeof` is `undefined`
6. Symbol (ES6) - `typeof` is `symbol`

> By default variable assignment, return variable and pass as parameter to function are handled **by value** technique.
>
> Javascript does not support `&` token for reference technique.

## Non-primitive types

1. Object - `typeof` is `object`
2. Array - `typeof` is `object`

> By default variable assignment, return variable and pass as parameter to function are handled **by share** technique.
