# Interfaces

Apart from basic or primitive types checking, you need to check the shape or structure of the simple or complex object, class and function. Then you can use interface to set type contract that object, class or function must follow.

```typescript
// must have shape of argument object passed to square function
interface SquareConfig {
  color: string;
  width: number;
}

function square(config: SquareConfig): void {
  // ...code
}

createSquare({color: "black", width: 100});
```

## Optional

```typescript
// color is optional
interface SquareConfig {
  color?: string;
  width: number;
}
```

## Readonly

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}
// can assign property for first time when object is created
let p1: Point = { x: 10, y: 20 };
// cannot modify after that
p1.x = 15; // error!
```

### Readonly array

```typescript
let a: ReadonlyArray<number> = [1, 2, 3];
a.push(23); // error

// you also cannot assign readonly array to variable but
// you can achieve it by using type assertion
let b = a; // error
let b = a as number[]; // ok
```

## Additional properties but not sure how many

```typescript
// SquareConfig can have any number of properties, and as
// long as they aren’t color or width and their types don’t matter.
interface SquareConfig {
  color?: string;
  width: number;
}
```

## Function's interface

```typescript
interface SearchFunc {
  (a: string, b: string): boolean;
}

// assign interface to variable and then use variable for function expression
let mySearch: SearchFunc;
mySearch = function(a: string, b: string) {
 let result = a.search(b);
 return result > -1;
}
```

## Indexable type

First, in javascript object key always treated as string even if you specify number key, javascript make it string behind the scene, therefore:

```typescript
let obj = {
  '100': 1,
  100: 2
};
obj['100']; // always 2 because in javascript object number 100 converted
            // to string type '100' and override the previous key value.
```

Second, in javascript when you assign object as key to another object then behind the scene javascript runs `toString()` method of object to cast it into string and the default `toString()` method cast any object into string `[object Object]`.

```typescript
let obj = {message:'Hello'}
let foo = {};
foo[obj] = 'World';
console.log(foo["[object Object]"]); // World
```

To avoid such a issues, you can use Indexable type. And typescript support two type of index signatures: `string` and `number`.

```typescript
//# For array
// you are telling compiler that array index must be type number
// and element type must be number.
interface Foo {
}
let arr: Foo = [1, 2, 3];

//# For object
interface Foo {
  // this line is not an element of object but telling compiler
  //that all the elements in the object has key of type string and
  // value of type number.
  [index: string]: number;
  x: number;  // this is an actual element
  y: number;  // this is an actual element
}

let obj: Foo = {
    x: 1,
    y: 2
}
```

## Class type

Implementing an interface to enforce class to follow certain contract set by an interface.

```typescript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date);
}

class Clock implements ClockInterface {
  currentTime: Date;
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) { }
}
```

## Extending interface

```typescript
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```
