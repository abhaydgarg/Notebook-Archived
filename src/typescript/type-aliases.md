# Type Aliases

Type aliases create a new name for a type. Aliasing doesn’t actually create a new type - it creates a new name to refer to that type.

Use `type` keyword.

```typescript
type stringAlias = string; // type alias for string

function getName(n: stringAlias): void {
  if (typeof n === "string") {
    console.log('Yes');
  }
}

// OR

type StrOrNum = string|number;

// Usage: just like any other notation
var sample: StrOrNum;
sample = 123;
sample = '123';

// Just checking
sample = true; // Error!
```
