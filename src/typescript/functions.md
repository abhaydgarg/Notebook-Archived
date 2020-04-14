# Functions

```typescript
function add(x: number, y: number): number {
    return x + y;
}

// OR

let add = function (x: number, y: number): number {
    return x + y;
}
```

## Optional & default parameters

1. Every parameter is assumed to be required by the function but it could be optional too.
2. The number of arguments given to a function has to match the number of parameters the function expects.
3. Optional parameters when not provided then they get `undefined` value by default.

```typescript
//# optional
function buildName(firstName: string, lastName? : string) {
  if (lastName)
    return firstName + " " + lastName;
  else
    return firstName;
}

let result1 = buildName("Bob"); // works correctly
let result2 = buildName("Bob", "Adams", "Sr."); // error, too many parameters
let result3 = buildName("Bob", "Adams"); // ah, just right

//# default
function buildName(firstName: string, lastName = "Smith") {
  return firstName + " " + lastName;
}

let result1 = buildName("Bob"); // works correctly now, returns "Bob Smith"
let result2 = buildName("Bob", undefined); // still works, also returns "Bob Smith"
let result3 = buildName("Bob", "Adams", "Sr."); // error, too many parameters
let result4 = buildName("Bob", "Adams"); // ah, just right
```

## Rest parameters

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

## Function overloading

This is useful for documentation + type safety purpose.

If you look at the code carefully you realize the meaning of a,b,c,d change based on how many arguments are passed in. Also the function only expects 1, 2 or 4 arguments. These constraints can be enforced and documented using function overloading. You just:

1. declare the function header multiple times,
2. the last function header is the one that is actually active within the function body but is not available to the outside world.

```typescript
// Overloads
function padding(all: number);
function padding(topAndBottom: number, leftAndRight: number);
function padding(top: number, right: number, bottom: number, left: number);
// Actual implementation that is a true representation of all
// the cases the function body needs to handle
function padding(a: number, b ? : number, c ? : number, d ? : number) {
  if (b === undefined && c === undefined && d === undefined) {
    b = c = d = a;
  } else if (c === undefined && d === undefined) {
    c = a;
    d = b;
  }
  return {
    top: a,
    right: b,
    bottom: c,
    left: d
  };
}

padding(1); // Okay: all
padding(1, 1); // Okay: topAndBottom, leftAndRight
padding(1, 1, 1, 1); // Okay: top, right, bottom, left

padding(1, 1, 1); // Error: Not a part of the available overloads
```
