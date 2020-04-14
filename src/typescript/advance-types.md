# Advance types

## Union type

Can be either of type.

```typescript
let x: number | string;
x = 1; // ok
x = 'hi'; //ok
x = false // error
```

## Intersection type

An intersection type combines several different types into a single supertype that includes the members from all participating types. Where a union type is “either type A or B,” an intersection type is “both type A and B.”

```typescript
interface IStudent {
  id: string;
  age: number;
}

interface IWorker {
  companyId: string;
}

type A = IStudent & IWorker;

let x: A;

x.age = 5;
x.companyId = 'CID5241';
x.id = 'ID3241';
```

## Literal type

**literal** = taking words in their usual or most basic sense without metaphor or exaggeration.

String literal types allow you to specify the exact value a string must have. In practice string literal types combine nicely with union types, type guards, and type aliases. You can use these features together to get enum-like behavior with strings.

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";

class UIElement {
  animate(dx: number, dy: number, easing: Easing) {
    if (easing === "ease-in") {
      // ...
    }
    else if (easing === "ease-out") {
    }
    else if (easing === "ease-in-out") {
    }
    else {
      // error! should not pass null or undefined.
    }
  }
}

let button = new UIElement();
button.animate(0, 0, "ease-in");
button.animate(0, 0, "uneasy"); // error: "uneasy" is not allowed here
```

TypeScript also supports boolean, numbers as literals, e.g.

```typescript
type OneToFive = 1 | 2 | 3 | 4 | 5;
type Bools = true | false;
```
