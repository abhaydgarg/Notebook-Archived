# Type guard

There are two types of operator provided by javascript to check type, which are `typeof` and `instanceof`.

```typescript
// instanceof example
class Student {
  study() {
  }
}

class Professor {
  teach() {
  }
}

function getPerson(n: number): Student | Professor {
  if (n === 1) {
    return new Student();
  } else {
    return new Professor();
  }
}

let person: Student | Professor = getPerson(1);

if (person instanceof Student) {
  person.study(); // ok
} else {
  person.teach(); // ok
}
```

Apart from `typeof` and `instanceof` you can also create own **custom type guard**.
To define a type guard, we simply need to define a function whose return type is a type predicate (the part of a sentence or clause containing a verb and stating something about the subject (e.g. went home in John went home ))

```typescript
/**
 * Just some interfaces
 */
interface Foo {
  foo: number;
  common: string;
}

interface Bar {
  bar: number;
  common: string;
}

/**
 * User Defined Type Guard!
 * Predicate = `arg is Foo`
 * Where: `arg` is name of argument or variable type to check
 * and `Foo` is a type
 */
function isFoo(arg: any): arg is Foo {
  return arg.foo !== undefined;
}

/**
 * Sample usage of the User Defined Type Guard
 */
function doStuff(arg: Foo | Bar) {
  if (isFoo(arg)) {
    console.log(arg.foo); // OK
    console.log(arg.bar); // Error!
  }
  else {
    console.log(arg.foo); // Error!
    console.log(arg.bar); // OK
  }
}

doStuff({ foo: 123, common: '123' });
doStuff({ bar: 123, common: '123' });
```
