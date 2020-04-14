# Basic types

```typescript
//# boolean
let isDone: boolean = false;

//# number
let decimal: number = 6;

//# string
let color: string = "blue";
let sentence: string = `Hello, my name is ${ fullName }

//# array - type[] or Array<type>
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];

//# tuple - array having mixed type
let x: [string, number]; // Declare a tuple type
x = ["hello", 10]; // Initialize it

//# enum
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
// By default, enums begin numbering their members starting at 0. You can change
// this by manually setting the value of one of its members.
// For example, we can start the previous example at 1 instead of 0:
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
// OR, even manually
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;

//# any - type of variables that we do not know its type
let notSure: any = 4;
notSure = "maybe a string instead";
// any in case of mixed array elements
let list: any[] = [1, true, "free"];

//# void - return type of function that return nothing
function warnUser(): void {
    alert("This is my warning message");
}

//# null and undefined
// you can assign null and undefined to any type
let num: number = undefined;
let str: string = null;
// --strictNullChecks flag, null and undefined can only be assigned to void type.
// In order to assign null and undefined to certain type then use union type
let a: number | undefined | null;

//# never - type of value that never occurs
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}
```

## null, undefined and --strictNullChecks

Effectively, `null` and `undefined` are valid values of every type. That means it’s not possible to stop them from being assigned to any type, even when you would like to prevent it.

```typescript
// `s` is of type string but later you can assign null or undefined to it
let s: string = "foo";
s = null; // ok
s = undefined; // ok
```

If you want to avoid assignment of `null` and `undefined` then you must on **--strictNullChecks** flag.

```typescript
// --strictNullChecks = on
let s: string = "foo";
s = null; // error
s = undefined; // error

// to use null or undefined then use union type
let s: string | undefined = "foo";
s = undefined; // ok
```

**Special case** - Optional parameters and properties

> With --strictNullChecks = on, an optional parameters and optional object properties are automatically adds `| undefined`.

```typescript
//# Optional Parameter
// compiler actually set type of `y` is 'number | undefined'
function f(x: number, y?: number) {
  return x + (y || 0);
}

f(1, 2); // ok
f(1); //ok
f(1, undefined); // ok
f(1, null); // error, 'null' is not assignable to 'number | undefined'

//# Optional Property
class C {
  a: number;
  b?: number;
}

let c = new C();
c.b = 13; //ok
c.b = undefined; // ok
c.b = null; // error, 'null' is not assignable to 'number | undefined'
```
