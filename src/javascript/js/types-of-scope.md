# Type of scopes

Scope defines the way JavaScript resolves a variable at run time. There is only two scopes in JavaScript -- global and function (local) scope. Moreover, it also deals with something called "scope chain" that makes closures possible.

**Global scope:** Not inside function.

**Local scope:** Inside function. Local scopes in JavaScript are created with Function Scope only, they aren't created by for or while loops or expression statements like if or switch.

> **Rule:** Function = New Local Scope

```js
// Global Scope
var myFunction = function () {
  // Local Scope A
  var myOtherFunction = function () {
    // Local Scope B
  };
};
```
