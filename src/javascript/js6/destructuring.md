# Destructuring

## Object destructuring

```js
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"

// ### 1. Destructing already defined variables ###
let node = {
        type: "Identifier",
        name: "foo"
    },
    type = "Literal",
    name = 5;

({ type, name } = node); // SEE - no let,var or const keyword but must be wrapped around ()

console.log(type);      // "Identifier"
console.log(name);      // "foo"

// ### 2. Default Values ###
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name, value } = node; // value key is missing in node object, so undefined is assigned

console.log(type);      // "Identifier"
console.log(name);      // "foo"
console.log(value);     // undefined

let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name, value = true } = node; // default value

console.log(type);      // "Identifier"
console.log(name);      // "foo"
console.log(value);     // true

// ### 3. Assigning to Different Local Variable Names ###
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type: localType, name: localName } = node;

console.log(localType);     // "Identifier"
console.log(localName);     // "foo"

// ### 4. Nested Object ###
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        }
    };

let { loc: { start }} = node;

console.log(start.line);        // 1
console.log(start.column);      // 1
```

### Do not forget the initializer

```js
// Initializer on right side of = is missing.

// syntax error!
var { type, name };

// syntax error!
let { type, name };

// syntax error!
const { type, name };
```

## Array destructuring

Array destructuring syntax is very similar to object destructuring; it just uses array literal syntax instead of object literal syntax.

```js
let colors = [ "red", "green", "blue" ];

let [ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"

// ### 1. Omit items ###
let colors = [ "red", "green", "blue" ];

let [ , , thirdColor ] = colors;

console.log(thirdColor);

// ### 2. Destructing already defined variables ###
let colors = [ "red", "green", "blue" ],
    firstColor = "black",
    secondColor = "purple";

[ firstColor, secondColor ] = colors; // SEE - no wrap around () as we do in object destructing.

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"

// ### 3. Default values ###
let colors = [ "red" ];

let [ firstColor, secondColor = "green" ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"

// ### 4. Nested ###
let colors = [ "red", [ "green", "lightgreen" ], "blue" ];

let [ firstColor, [ secondColor ] ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"

// ### 4. Rest items ###
let colors = [ "red", "green", "blue" ];

let [ firstColor, ...restColors ] = colors;

console.log(firstColor);        // "red"
console.log(restColors.length); // 2
console.log(restColors[0]);     // "green"
console.log(restColors[1]);     // "blue"
```
