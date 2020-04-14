# First class function vs High order function

**First class function:** Function is treated as value, that you can assign a function into a variable, pass it as parameter or  return it.

**High order function:** That take at least one first class function as a parameter or return function as result.

**What is the difference?**

A *higher order function* is a function that may receive a function as parameter or even return a function. In JavaScript that is possible cause functions are *first class citizens*, that means that they can be passed as parameters, returned, assigned to variables etc. So you could say that high order function is sub set of first class function.

**Great usage of having ability in language to have high order function and first class function.**

Let's say I want to convert this array of strings to an array of integers, each of which represents the character-length of the starting string:

Goal: `["cat", "it", "banana", "fish", "do", "dodo"] --> [3, 2, 6, 4, 2, 4]`

do this with `Array.map`:

`var lengths = ["cat", "it", "banana", "fish", "do", "dodo"].map( ... );`

I need to give the map function another function. The Javascript interpreter will run that function for each member of the array, each time giving it one of the array's members. It will build an output array (which will get stored in the variable tlengths), using whatever the input function returns.

So, for instance, if I can give map this as a parameter:

`function findLength(str) { return str.length; }`

Putting it all together, we get this:

```js
function findLength(str) { return str.length }

var lengths = ["cat", "it", "banana", "fish", "do", "dodo"].map(findLength);
console.log(lengths); // [3, 2, 6, 4, 2, 4]
```

Let's imagine Javascript didn't have such a function. We could build it ourselves, thus creating our own higher-order function:

```js
function map(originalArray, itemFunction) {
   var newArray = [];
   var newValue;
   var i;
   var len;
   for (i = 0, len = originalArray.length; i < len; i++) {
     newValue = itemFunction(originalArray[ i ]);
     newArray.push(newValue);
  }
  return newArray();
}
```

To meet my original goal, I would call my custom higher-order function like this:

```js
function findLength(str) { return str.length }

var lengths = map(["cat", "it", "banana", "fish", "do", "dodo"], findLength);
```

This way we could make things work with a few lines of code.
