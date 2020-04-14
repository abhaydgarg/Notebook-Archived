# Undefined value in document

If document has `undefined` value of any key/field then mongodb exclude that key/field from the result.

```js
/* Document */
{
  a: 1,
  b: undefined,
  c: null
}

/* fetch document by find query */

{
  a: 1,
  c: null
}
```
