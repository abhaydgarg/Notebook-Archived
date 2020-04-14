# Type assertion

It is also be said `type casting` but as in other languages type casting actually change the nature of variable for example, say you have variable of type string and you type cast it to number then internally compiler changes the structure of variable.
But in the case of typescript no such thing happens, it just a way of telling compiler that as a programmer i know the type of variable and do not do type checking.

```typescript
// say we have variable with type any and the string value is assigned to it.
// So we can instruct the compiler that you do not need to do type checking
// on variable but just use it as string.
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
//OR
let strLength: number = (someValue as string).length;
```
