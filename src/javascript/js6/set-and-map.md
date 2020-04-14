# Why sets and maps

In ES5, object is used as data structure to store data. But using object as map in ES5 has few limitations.

## Object's keys must be a string

If you provide any other type other than string ES5 internally changed the type to string before assigning key to object. Therefore key `5` and key `'5'` are same in ES5.

```js
// This example assigns the string value "foo" to a numeric key of 5. Internally, that
// numeric value is converted to a string, so obj["5"] and obj[5] actually reference
// the same property. That internal conversion can cause problems when you want to use
// both numbers and strings as keys.

let obj = {};
obj[5] = "foo";
console.log(obj["5"]);      // "foo"

// Another problem arises when using objects as keys. Here, obj[key2] and obj[key1]
// reference the same value. Because both are object and before assigning key JS convert
// them to string which is "[object Object]", there fore both key1 and key2 has value "[object Object]".

let obj = {},
    key1 = {},
    key2 = {};

map[key1] = "foo";

console.log(map[key2]);     // "foo"
```

## Determine object's properties

Objects weren't designed to be used as collections, and as a result there's no efficient way to determine how many properties an object has. When you loop over an object's properties, you also get its prototype properties. You could add the `iterable` property to all objects but not all objects are meant to be used as collections. You could use the `for...in` loop and the `hasOwnProperty()` method, but this just a workaround. When you loop over an object's properties, the properties won't necessarily be retrieved in the same order they were inserted.

## Object built-in methods

Objects have built in methods like `constructor`, `toString`, and `valueOf`. If one of these was added as a property, it could cause collisions. You could use `Object.create(null)` to create a bare object (which doesn't inherit from `object.prototype`), but, again, this is just a workaround.

## Sets

It's a collection for _unique_ values. The values could be also a primitives or object references.

```js
// ### Add

// does not change the type, so 5 and '5' are two different values.
let set = new Set();
set.add(5);
set.add("5");
console.log(set.size);    // 2

// key1 and key2 are not converted to strings, they count as two unique items in the set.
// (Remember, if they were converted to strings, they would both be equal to "[Object object]".)
let set = new Set(),
    key1 = {},
    key2 = {};

set.add(key1);
set.add(key2);
console.log(set.size);    // 2

// ### Does not allow duplicate values. Duplicate values are just ignored.

let set = new Set();
set.add(5);
set.add("5");
set.add(5);     // duplicate - this is ignored
console.log(set.size);    // 2

// ### Initialize a set using an array, and the Set constructor
// will ensure that only unique values are used.

let set = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
console.log(set.size);    // 5

// ### has()

let set = new Set();
set.add(5);
set.add("5");

console.log(set.has(5));    // true
console.log(set.has(6));    // false

// ### Removing Values

let set = new Set();
set.add(5);
set.add("5");
set.delete(5); // remove single value

set.clear(); // empty whole set

// ### Converting a Set to an Array

// This approach is useful when you already have an array and want
// to create an array without duplicates.
let set = new Set([1, 2, 3, 3, 3, 4, 5]),
    array = [...set];
console.log(array);             // [1,2,3,4,5]
```

### Weak sets

The sets we have discussed above is also called `strong sets` because the way it sores the object reference. Even if you remove the reference from original object, sets keep it. Therefore, original object cannot garbage collected to free memory.

```js
let set = new Set(), // strong sets
    key = {}; // original object

set.add(key);

// eliminate original reference
key = null;

// get the original reference back
key = [...set][0];

// Setting key to null clears one reference of the key object, but another remains inside set.
// You can still retrieve key by converting the set to an array with the spread operator and accessing
// the first item. That result is fine for most programs, but sometimes, it’s better
// for references in a set to disappear when all other references disappear.
// For instance, if your JavaScript code is running in a web page and wants to keep
// track of DOM elements that might be removed by another script, you don’t want your code holding
// onto the last reference to a DOM element. (That situation is called a memory leak.)
```

To alleviate such issues, ECMAScript 6 also includes _weak sets_

1. Weak sets only store object references.
2. Primitive values are not permitted.

```js
let set = new WeakSet(),
    key = {}; // original object

set.add(key);

// eliminate original reference
key = null;

// get the original reference back
key = [...set][0]; // null
```

#### How Weak sets differ from strong(regular) sets

1. In a `WeakSet` instance, the `add()` method, `has()` method, and `delete()` method all throw an error when passed a non-object.
2. Weak sets are not iterables and therefore cannot be used in a `for-of` loop.
3. Weak sets do not expose any iterators (such as the `keys()` and `values()` methods), so there is no way to programmatically determine the contents of a weak set.
4. Weak sets do not have a `forEach()` method.
5. Weak sets do not have a `size` property.

> In general, if you only need to track object references, then you should use a weak set instead of a regular set.

## Maps

Maps are a store for **key** / **value** pairs. Key and value could be a primitives or object references.

```js
// ### Add and Get

let map = new Map();
map.set("title", "Understanding ES6");
map.set("year", 2016);

console.log(map.get("title"));      // "Understanding ES6"
console.log(map.get("year"));       // 2016

// get() return undefined if key does not exist

// ### You can also use objects as keys, which isn’t possible when
// using object properties to create a map in the ES5

let map = new Map(),
    key1 = {},
    key2 = {};

map.set(key1, 5);
map.set(key2, 42);

console.log(map.get(key1));         // 5
console.log(map.get(key2));         // 42
```

* `has(key)` - Determines if the given key exists in the map
* `delete(key)` - Removes the key and its associated value from the map
* `clear()` - Removes all keys and values from the map
* `size` - number of key-value pair.

### Add a lot of data to a map at one time

Also similar to sets, you can initialize a map with data by passing an array to the `Map` constructor. Each item in the array must itself be an array where the first item is the key and the second is that key's corresponding value. The entire map, therefore, is an array of these two-item arrays, for example:

```js
let map = new Map([["name", "Nicholas"], ["age", 25]]);

console.log(map.get("name"));   // "Nicholas"
console.log(map.get("age"));    // 25
```

### Weak maps

Concept is similar to sets regarding weak object references to promote garbage collection. In weak maps, every key must be an object (an error is thrown if you try to use a non-object key), and those object references are held weakly so they don't interfere with garbage collection. It's

**Important** to note that only weak map keys, and not weak map values, are weak references. An object stored as a weak map value will prevent garbage collection if all other references are removed.

```js
let map = new WeakMap(),
    element = document.querySelector(".element");

map.set(element, "Original");

let value = map.get(element);
console.log(value);             // "Original"

// remove the element
element.parentNode.removeChild(element);
element = null;

// the weak map is empty at this point
```

#### Weak map uses and limitations

When deciding whether to use a weak map or a regular map, the primary decision to consider is whether you want to use only object as key. Anytime you're going to use only object as key, then the best choice is a weak map. That will allow you to optimize memory usage and avoid memory leaks by ensuring that extra data isn't kept around after it's no longer accessible.

Keep in mind that weak maps give you very little visibility into their contents, so you can't use the `forEach()` method, the `size`property, or the `clear()` method to manage the items. If you need some inspection capabilities, then regular maps are a better choice. Just be sure to keep an eye on memory usage.
