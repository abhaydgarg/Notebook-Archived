# Async await

## Async

> It always return Promise.

The word “async” before a function means one simple thing: a function always returns a promise. Even If a function actually returns a non-promise value, prepending the function definition with the “async” keyword directs Javascript to automatically wrap that value in a resolved promise.

For instance, the code above returns a resolved promise with the result of `1`, let’s test it:

```js
async function f() {
  return 1;
}

console.log(f()); // Promise {<resolved>: 1}
```

## Await

> Works only inside async function.

## Control flow using example

```js
function main() {
  return new Promise( resolve => {
    console.log(3);
    resolve(4);
    console.log(5);
  });
}

async function f(){
    console.log(2);
    let r = await main();
    console.log(r);
}

console.log(1);
f();
```

```
1
2
3
5
6
// Async happened, await for main()
4
```
