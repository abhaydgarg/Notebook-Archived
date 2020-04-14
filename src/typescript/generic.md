# Generic

Generic programming allows algorithms to be written in way that allows the types to be specified later. Able to create a component that can work over a variety of types rather than a single one. Components that you can create are `generic function`, `generic interfaces` and `generic class`.

## Generic function

To make a function generic, you add a type parameter enclosed in angle brackets (< >) immediately after the function name. When you call a generic function, you can specify the type argument by placing it in angle brackets after the function name. If the type can be inferred (e.g., by inspecting the types of the arguments passed to the function), the type argument becomes optional.

```typescript
function reverse < T > (list: T[]): T[] {
  const reversedList: T[] = [];
  for (let i = (list.length - 1); i >= 0; i--) {
    reversedList.push(list[i]);
  }
  return reversedList;
}
const letters = ['a', 'b', 'c', 'd'];
const reversedLetters = reverse < string > (letters); // d, c, b, a
const numbers = [1, 2, 3, 4];
const reversedNumbers = reverse < number > (numbers); // 4, 3, 2, 1
```

## Generic interface

To make a generic interface, the type parameters are placed directly after the interface name.

For example: Repository interface that has two type parameters representing the type of a domain object and the type of an ID for that domain object. These type parameters can be used as annotations anywhere within the interface declaration.
When the CustomerRepository class implements the generic interface, it supplies the concrete Customer and CustomerId types as type arguments. The body of the CustomerRepository class is checked to ensure that it implements the interface based on these types.

```typescript
interface Repository < T, TId > {
  getById(id: TId): T;
  persist(model: T): TId;
}

class CustomerId {
  constructor(private customerIdValue: number) {}
  get value() {
    return this.customerIdValue;
  }
}
class Customer {
  constructor(public id: CustomerId, public name: string) {}
}

class CustomerRepository implements Repository < Customer, CustomerId > {
  constructor(private customers: Customer[]) {}

  getById(id: CustomerId) {
    return this.customers[id.value];
  }
  persist(customer: Customer) {
    this.customers[customer.id.value] = customer;
    return customer.id;
  }
}
```

## Generic class

If generic interfaces can save some duplication in your code, generic classes can save even more by supplying a single implementation to service many different type scenarios. The type parameters follow the class name and are surrounded by angle brackets. The type parameter can be used to annotate method parameters, properties, return types, and local variables within the class.

```typescript
class GenericsExample < T > {
  value: T;
  setValue(value: T) {
    this.value = value;
  }
  getValue(): T {
    return this.value;
  }
}

var gen1 = new GenericsExample < string > ();
gen1.setValue("Hello World");
console.log(gen1.getValue()); // Hello World

var gen2 = new GenericsExample < number > ();
gen2.setValue(1);
console.log(gen2.getValue()); // 1

var gen3 = new GenericsExample < boolean > ();
gen3.setValue(true);
console.log(gen3.getValue()); // true
```

## Constraint

A type constraint can be used to limit the types that a generic function, interface, or class can operate on.

```typescript
interface HasName {
  name: string;
}

class Personalization {
  static greet < T extends HasName > (obj: T) {
    return 'Hello ' + obj.name;
  }
}
// if greet() method's parameter object does not contain `name`
// property then compiler issue an error.
```
