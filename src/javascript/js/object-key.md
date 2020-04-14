# Object key

## Key typecast to string

Javascript always change the key to `string` behind the scene. Therefore, `obj['5']` and `obj[5]` are same.

## Dot notation

```js
var obj = {};

//assign property like

obj.p.p1 = 1 //error p is undefined because you have not initialise
            //or given any value to obj.p yet, therefore obj.p is undefined.

But can do: // here obj.p is initialized with {} first

obj.p = {
 p1: 1,
 p2: 2
};
```

## . vs []

JavaScript Valid Variables.

**Start**: `[a-z], $, _`

**Contains**: `[a-z], [0-9], $, _`

But sometimes, in JSON, when we have property name containing space or any other character which is not acceptable for valid variable name in JavaScript, the JSON wrap it in quote either single or double. In this case, you cannot access it using dot but using `[]`.

```js
var obj = {
  x: 1,
  'y': 2,
  'My Name': 'abhay'
};
obj.x //1
obj.y //2

//the only way to access the 'My Name' property
obj['My Name'] //abhay
obj.'My Name' // not allowed
```

_Dot Notation:_ property must be a valid JavaScript identifier (variable). [] Notation: The string does not have to be a valid identifier; it can have any value.

**In [] notation, you can give variable name in place of property. Like PHP.**

```js
var x = 'name';
var obj = {
  'name': 'abhay'
};

obj[x] //abhay - because x is replaced with 'name'.
```

### How to work with dynamic keys

In **ES6**, we have `[]` notation to manipulate dynamic keys. So at the time of defining object we can use `[]` to use dynamic keys.

```js
var name = 'x'; // name value coming from DB and we do not know it before hand but
               // whatever would be the value, we want to make it an object key.
var obj = {
 [name]: 'hi there' // [name] replaces with 'x'
};

obj.x // 'hi there'
```

In **ES5**, we have to construct an empty object and then use dynamic keys. We cannot do it at the time of defining an object.

```js
// above example
var name = 'x';
var obj = {};
obj[name] = 'hi there'; // [name] replaces with 'x'

obj.x // 'hi there'

// more example
var userID = 1;
var setData = {}; // empty object
setData[userID] = { // use object key dynamically. userID replaces with 1. So actually key is 1 not userID.
    "w": true,
    "r": true
  };

db.getCollection('User').update({
    "_id": id
  }, {
    $set: setData
  });
```

### Should object keys enclose in quotes

If working with dynamic keys where you do not know the key before hand then in order to be safe try to enclose it in quotes.

If you follow `camelCase` or valid javascript identifier name then there is no need to enclose keys.

> If the name of key matches the variable in a scope then nothing happens. Javascript do not try to replace the key with the same variable name but it would replace the value of key if variable with same name found in a scope. See example below:

```js
var k = '1', z = '2';
var obj = {
  k: k, // value k replaced by 1 not key although variable k has same name as key k.
  z: z
};
```
