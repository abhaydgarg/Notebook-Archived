# Type coercion

> Javascript does implicit type coercion.

Explicit type coercion can be done in Javascript for example `String()` or `Number()`. Below are the situations where implicit type coercion can occur.

> `==` trigger type coercion if operands are of different types.

> `===` does not trigger type coercion if operands either of same type or different type.

## Two scenarios when implicit coercion triggers

1. When you apply **operators to values of different types**, like
`1 == null`, `2/’5'`, `null + new Date()`.

2. It can be triggered by the surrounding **context**, like with `if (value) {…}`, where `value` is coerced to boolean.

## Three type of implicit coercion

**You need to be clear which trigger either String, Boolean or Numeric conversion.**

### String conversion

To explicitly convert values to a string apply the `String()` function. Implicit coercion is triggered by the binary `+` operator, when any operand is a string:

```js
String(123) // explicit
123 + ''    // implicit
```

### Boolean conversion

To explicitly convert a value to a boolean apply the `Boolean()` function.
Implicit conversion happens in logical context, or is triggered by logical operators (`||` `&&` `!`) .

```js
Boolean(2)          // explicit
if (2) { ... }      // implicit due to logical context
!!2                 // implicit due to logical operator
2 || 'hello'        // implicit due to logical operator
```

**Note**: Logical operators such as `||` and `&&` do boolean conversions internally, but [actually return the value of original operands](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Logical_operators), even if they are not boolean.

#### Stuff to remember

```js
Boolean('')           // false
Boolean(0)            // false
Boolean(-0)           // false
Boolean(NaN)          // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(false)        // false

// Empty object and arrays are truthy values
Boolean({})             // true
Boolean([])             // true
Boolean(Symbol())       // true
Boolean(function() {})  // true
```

### Numeric conversion

For an explicit conversion just apply the `Number()` function.

Implicit conversion is tricky, because it’s triggered in more cases:

- comparison operators (`>`,  `<`,  `<=`,`>=`)
- bitwise operators (`|`  `&`  `^`  `~`)
- arithmetic operators (`-`  `+`  `*`  `/`  `%`)
- unary  `+`  operator
- loose equality operator `==`  (incl. `!=`)

```js
Number('123')   // explicit
+'123'          // implicit
123 != '456'    // implicit
4 > '5'         // implicit
5/null          // implicit
true | 0        // implicit
```

```js
Number(null)                   // 0
Number(undefined)              // NaN
Number(true)                   // 1
Number(false)                  // 0
Number(" 12 ")                 // 12
Number("-12.34")               // -12.34
Number("\n")                   // 0
Number(" 12s ")                // NaN
Number(123)                    // 123
```

#### Special rules when dealing with numeric conversion

1. `+` does not trigger numeric conversion, when any operand is a string.

2. `==` does not trigger numeric conversion when both operands are strings.

3. When numeric conversion trigger by `==`; `null` and `undefined` are not converted to numeric like `Number(null) => 0`, it remains `null`.

## Type coercion for objects

So far, we’ve looked at type coercion for primitive values.
euivalent
When it comes to objects and engine encounters expression like `[1] + [2,3]`, first it **needs to convert an object to a primitive value**, which is then converted to the final type. And still there are only three types of conversion: numeric, string and boolean.

Objects are converted to primitives via the internal  `[[ToPrimitive]]`  method, which is responsible for both numeric and string conversion.

Here is a pseudo implementation of  `[[ToPrimitive]]`  method:

```js
function ToPrimitive(input, preferredType){

  switch (preferredType){
    case Number:
      return toNumber(input);
      break;
    case String:
      return toString(input);
      break
    default:
      return toNumber(input);
  }

  function isPrimitive(value){
    return value !== Object(value);
  }

  function toString(){
    if (isPrimitive(input.toString())) return input.toString();
    if (isPrimitive(input.valueOf())) return input.valueOf();
    throw new TypeError();
  }

  function toNumber(){
    if (isPrimitive(input.valueOf())) return input.valueOf();
    if (isPrimitive(input.toString())) return input.toString();
    throw new TypeError();
  }
}
```

Both numeric and string conversion make use of two methods of the input object: `valueOf` and `toString` . Both methods are declared on `Object.prototype` and thus available for any derived types, such as `Date`, `Array`, etc.

In general the algorithm is as follows:

1. If input is already a primitive, do nothing and return it.

2. Call  `input.toString()`, if the result is primitive, return it.

3. Call  `input.valueOf()`, if the result is primitive, return it.

4. If neither  `input.toString()`  nor  `input.valueOf()`  yields primitive, throw  `TypeError`.

Numeric conversion first calls  `valueOf`  (3) with a fallback to  `toString`  (2). String conversion does the opposite:  `toString`  (2) followed by  `valueOf`  (3).

## `NaN` does not equal to anything even itself.

## Examples

Think - Which expression triggers which implicit coercion. Either String, Boolean or Numeric.

```js
// Binary + operator triggers numeric conversion for true and false
true + false             // 1

// Arithmetic division operator / triggers numeric conversion for string '6'
12 / "6"                 // 2

// Operator + has left-to-right associativity, so expression "number" + 15 runs first.
// Since one operand is a string, + operator triggers string conversion for the number 15.
// On the second step expression "number15" + 3 is evaluated similarly.
"number" + 15 + 3        // 'number153'

// Expression 15 + 3 is evaluated first. No need for coercion at all, since both operands
// are numbers. On the second step, expression 18 + 'number' is evaluated, and since one
// operand is a string, it triggers a string conversion.
15 + 3 + "number"        // '18number'

// Comparison operator > triggers numeric conversion for [1] and null
[1] > null               // true

// Unary + operator has higher precedence over binary + operator. So +'bar' expression
// evaluates first. Unary plus triggers numeric conversion for string 'bar'. Since the
// string does not represent a valid number, the result is NaN. On the second step,
// expression 'foo' + NaN is evaluated.
"foo" + + "bar"          // 'fooNaN'

// == operator triggers numeric conversion, string 'true' is converted to NaN, boolean true is converted to 1.
'true' == true           // false
false == 'false'         // false

// == usually triggers numeric conversion. null equals to null or undefined only,
// and does not equal to anything else.
null == ''               // false

// trigger boolean conversion of both
!!"false" == !!"true"    // true

// == operator triggers a numeric conversion for an array. Array’s valueOf() method
// returns the array itself, and is ignored because it’s not a primitive. Array’s
// toString() converts ['x'] to just 'x' string.
['x'] == 'x'             // true

// trigger numeric conversion
[] + null + 1            // 'null1'

// No coercion because both are same type
[1,2,3] == [1,2,3]       // false

// This one is better explained step by step according to operator precedence.
// !+[]+[]+![]
// ==> (!+[]) + [] + (![])
// ==> !0 + [] + false
// ==> true + [] + false
// ==> true + '' + false
// ==> 'truefalse'
!+[]+[]+![]              // 'truefalse'

// - operator triggers numeric conversion for Date. Date.valueOf()
// returns number of milliseconds since Unix epoch.
new Date(0) - 0          // 0

// + operator triggers default conversion. Date assumes string conversion
// as a default one, so toString() method is used, rather than valueOf().
new Date(0) + 0          // 'Thu Jan 01 1970 02:00:00(EET)0'
```
