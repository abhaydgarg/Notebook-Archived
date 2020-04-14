# Class as type

You can use class as **type** ("shape/structure") like an interface.

```typescript
class Point {
    x: number;
    y: number;
    multiply(): number;
}

let point: Point = {
  x: 1,
  y: 2,
  multiply: function() {
    return this.x * this.y;
  }
};
```

**Class can also be extended with `interface`.**

```typescript
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

## Should I be using an interface or a class for `this` type annotation

Let’s start off with an example in order to focus in on what we are trying to understand in this post:

```typescript
fetch('https://jameshenry.blog/foo/bar')
    .then((response) => {
        console.log(response.status) // Some HTTP status code, such as 200
    })
```

If we let TypeScript take a look at this code as it is now, it would be forced to infer the type of the response parameter as any. There is no way for it to know, just by analysing the code, what the type should be.

At this point, to increase the type safety of our program, we would want to add our own explicit type annotation to response, in order to tell the TypeScript compiler what we believe the type should be:

```typescript
fetch('https://jameshenry.blog/foo/bar')
    .then((response: Response) => {
        console.log(response.status)
    })
```

Now we have reached the central question that; Should our new `Response` type be defined as an `interface` or a `class`?

In TypeScript, an interface is a way for us to take particular shape and give it a name, so that we can reference it later as a type in our program.

> **“If it looks like a duck, and quacks like a duck, it’s a duck.”**

In other words, we are determining if something can be classified as a particular type by looking at whether or not it has the required characteristics/structure/shape. Let’s call it **“shape”** from now on.

```typescript
// A duck must have...
interface Duck {
    // ...a `hasWings` property with the value `true` (boolean literal type)
    hasWings: true
    // ...a `noOfFeet` property with the value `2` (number literal type)
    noOfFeet: 2
    // ...a `quack` method which does not return anything
    quack(): void
}
```

From now on in our TypeScript code, if we want to make sure something is a duck (which really means, it “implements our Duck interface”), all we need to do is reference its type as `Duck`.

```typescript
// This would pass type-checking!
const duck: Duck = {
    hasWings: true,
    noOfFeet: 2,
    quack() {
        console.log('Quack!')
    },
}

// This would not pass type-checking as it does not
// correctly implement the Duck interface.
const notADuck: Duck = {}
// The TypeScript compiler would tell us
// "Type '{}' is not assignable to type 'Duck'.
// Property 'hasWings' is missing in type '{}'."
```

Now we understand how interfaces can help TypeScript catch more potential issues in our code at compile time, but there is one more critical feature of interfaces that we need to keep in mind:

> An interface is only used by TypeScript at compile time, and is then removed. Interfaces do not end up in our final JavaScript output.

Let’s complete the section on interfaces by finally defining our dead simple Response type as an `interface`:

```typescript
interface Response {
    status: number // Some HTTP status code, such as 200
}

fetch('https://jameshenry.blog/foo/bar')
    .then((response: Response) => {
        console.log(response.status)
    })
```

_Typescript transpile code into:_

```js
// interface code is removed in final output

fetch('https://jameshenry.blog/foo/bar')
    .then(function (response) {
    console.log(response.status);
});
```

**Let’s now take our example and redefine our Response type as a class instead of an interface:**

```typescript
class Response {
    status: number // Some HTTP status code, such as 200
}

fetch('https://jameshenry.blog/foo/bar')
    .then((response: Response) => {
        console.log(response.status)
    })
```

As we can see, in this case it is simply a matter of changing interface to class, and if we pass our code through the TypeScript compiler, we will still have the exact same level of type-safety and produce no compile time errors! So far, the behaviour is identical.>

> The real difference comes when we consider our compiled JavaScript output.
>
> **The biggest difference between a class and an interface is that a class provides an implementation of something, not just its shape.**

_Typescript transpile code into:_

```js
var Response = (function () {
    function Response() {
    }
    return Response;
}());
fetch('https://jameshenry.blog/foo/bar')
    .then(function (response) {
    console.log(response.status);
});
```

Very different! As we can see, our class is being transpiled into its ES5-compatible function form, and is now an unnecessary part of our final JavaScript application. If we had a large application, and repeated this pattern of using `classes` as model type annotations, then we could end up adding a lot of extra bloat to our users’ bundles.
