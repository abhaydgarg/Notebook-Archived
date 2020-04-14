# Terminology

The essential concepts in RxJS which solve async event management are:

* **Observable**: represents the idea of an invokable collection of future values or events.
* **Observer**: is a collection of callbacks that knows how to listen to values delivered by the Observable.
* **Subscription**: represents the execution of an Observable, is primarily useful for cancelling the execution.
* Operators: are pure functions that enable a functional programming style of dealing with collections with operations like map, filter, concat, flatMap, etc.
* **Subject**: is the equivalent to an EventEmitter, and the only way of multicasting a value or event to multiple Observers.
* **Schedulers**: are centralized dispatchers to control concurrency, allowing us to coordinate when computation happens on e.g. setTimeout or requestAnimationFrame or others.

Functional programming is about writing pure functions, about removing hidden inputs and outputs as far as we can, so that as much of our code as possible just describes a relationship between inputs and outputs.

## Functional programming

It is the process of building software by composing **pure functions**, avoiding **shared state**, **mutable data**, and **side-effects**.

### Pure function

They are the simplest reusable building blocks of code in a program. Perhaps the most important design principle in computer science is KISS (Keep It Simple, Stupid). Pure functions are stupid simple in the best possible way.

Foundation of **functional programming**.

Pure functions are also extremely independent which makes them great candidates for **parallel processing.**

Pure functions are referentially transparent, we only need to compute their output once for given inputs. Caching and reusing the result of a computation is called **memoization**, and can only be done safely with pure functions.

_A pure function is a function which:_

#### Given the same input, will always return the same output

A pure function doesn’t depend on and doesn’t modify the states of variables out of its scope. Its execution doesn’t depend on the state of the system.

```js
// Take slice and splice. They are two functions that do the exact same thing
// - in a vastly different way. We say `slice` is pure because it returns the same
// output per input every time, guaranteed. On other hand `splice`, chew up its array.

var xs = [1, 2, 3, 4, 5];

//## pure - `slice` does not modify the state of `xs` which is a outside of its scope
xs.slice(0, 3);
//=> [1, 2, 3]

xs.slice(0, 3);
//=> [1, 2, 3]

xs.slice(0, 3);
//=> [1, 2, 3]


//## impure - `splice` does modify the state of `xs` which is a outside of its scope
xs.splice(0, 3);
//=> [1, 2, 3]

xs.splice(0, 3);
//=> [4, 5]

xs.splice(0, 3);
//=> []
```

```js
//## impure - `checkAge` depends on the mutable variable `minimum` to determine the result.
// In other words, it depends on system state.

var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};

//## pure
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```

#### Produces no side effects

It means that it can’t alter any external state.

**What is side effects?**

Any interaction with the world outside of a function is a side effect.

A side effect is a change of system state that occurs during the calculation of a result. For example; changing the file system, inserting a record into a database, making an http call etc.

_**Immutability:**_ JavaScript’s object arguments are references, which means that if a function were to mutate a property on an object or array parameter, that would mutate state that is accessible outside the function. **Pure functions must not mutate external state.**

Side effects disqualify a function from being pure. And it makes sense: pure functions, by definition, must always return the same output given the same input, which is not possible to guarantee when dealing with matters outside our local function.
