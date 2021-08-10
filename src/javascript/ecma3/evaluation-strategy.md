# Evaluation strategy

Many programmers are assured that objects in JavaScript (as well as in some other habitual languages) are passed in function _by reference_ while values of primitive types are passed _by value_. However, how much exact is this terminology and how much true this description.

## General theory

Briefly notice that in the general theory there are two kinds of evaluation strategy: _strict_, meaning that arguments are calculated before their application and _non-strict_, meaning, as a rule, calculation of arguments on demand when they are required (so-called “lazy” calculations).

However, here we consider basic strategies of passing arguments to function which are important for understanding from the point of ECMAScript view.

And to begin with, it is necessary to notice that in ECMAScript, as well as in many other languages (for example, C, Java, Python, Ruby, other) _strict_ strategy arguments passing is used.

Also the order in which arguments are being evaluated is important — in ECMAScript it is left-to-right. In other languages and their implementations the reverse evaluation order (i.e. right-to-left) can be used.

Strict passing strategy is also subdivided into several strategies, most important of which we consider in detail.

## by value

This type of strategy is well-known by many programmers. The value of argument here is the _copy of value_ of the object passed by the caller. The changes made to the parameter inside the function _do not influence_ the passed object outside. Generally, there is an allocation of new memory (thus, concrete implementation in this discussion is not so essential — it can be a stack or dynamic memory) in which value of external object is copied, and exactly value from this new area of memory is used inside the function.

```pascal
bar = 10

procedure foo(barArg):
  barArg = 20;
end

foo(bar)

// changes inside foo didn't affect
// on bar which is outside
print(bar) // 10
```

However, this strategy causes the big performance issue in a case when function argument is not primitive value, but complex structure, an object. That exactly occurs, for example, in C/C++ when the structure is passed by value to function — it is _completely copied_.

Let’s describe the general example which we will use at the description of the following evaluation strategies. Let abstract procedure accepts two arguments: value of an object and also a boolean flag, whether it is necessary to change the object’s value completely (to assign a new value), or just to change only properties of the object.

```pascal
bar = {
  x: 10,
  y: 20
}

procedure foo(barArg, isFullChange):

  if isFullChange:
    barArg = {z: 1, q: 2}
    exit
  end

  barArg.x = 100
  barArg.y = 200

end

foo(bar)

// with call by value strategy,
// object outside has not been changed
print(bar) // {x: 10, y: 20}

// the same with full change
// (assigning the new value)
foo(bar, true)

//also, there were no changes made
print(bar) // {x: 10, y: 20}, but not {z: 1, q: 2}
```

## by reference

In turn _by reference_ evaluation strategy (which is also well-known) receives _not a value copy_, but the _implicit reference to object_, i.e. the address directly related with object from the outside. Any change of parameter inside the function (assignment of new value or change of properties) _is affected on object outside_ because the exact address of this object is related with formal parameter, i.e. an argument is kind of _alias_ for the object from the outside.

```pascal
// definition of foo procedure see above

// having the same object
bar = {
  x: 10,
  y: 20
}

// results of foo procedure
// with call by reference
// are the following

foo(bar)

// property values of the object are changed
print(bar) // {x: 100, y: 200}

// assigning of the new value
// is also affected on the object
foo(bar, true)

// bar now references the new object
print(bar) // {z: 1, q: 2}
```

This strategy allows more effectively to pass complex objects, e.g. big structures with a considerable quantity of properties.

## by share

Alternative names of this strategy are **“call by object”** or **“call by object-sharing”**.

The main point of this strategy is that function receives the _copy of the reference_ to object. This reference copy is associated with the formal parameter and is its value.

Regardless the fact that the concept of the _reference_ in this case appears, this strategy _should not be treated as call by reference_ (though, in this case the majority makes a mistake), because the value of the argument is _not the direct alias_, but the _copy of the address_.

The main difference consists that _assignment of a new value to argument inside the function does not affect object outside_ (as it would be in case of _call by reference_). However, because formal parameter, having an address copy, gets access to the same object that is outside (i.e. the object from the outside completely _was not copied_ as would be in case of _call by value_), _changes of properties of local argument object — are reflected in the external object_.

```pascal
// definition of foo procedure see above

// again, the same object structure
bar = {
  x: 10,
  y: 20
}

// call by sharing
// affects on object
// in the following manner

foo(bar)

// values of object properties are changed
print(bar) // {x: 100, y: 200}

// but with full change of object
// there is no changes
foo(bar, true)

// still the same from the previous call
print(bar) // {x: 100, y: 200}
```

### ECMAScript implementation

Now we know which evaluation strategy for passing objects as arguments is used in ECMAScript — _call by sharing_: changes of properties of the argument are affected outside, assignment of the new value — do not influence external object.

```js
var foo = {x: 10, y: 20};
var bar = foo;

alert(bar === foo); // true

bar.x = 100;
bar.y = 200;

alert([foo.x, foo.y]); // [100, 200]
```

I.e. two identifiers (name bindings) are bound to the same object in memory, _sharing_ this object:

`foo value: addr(0xFF) => {x: 100, y: 200} (address 0xFF) <= bar value: addr(0xFF)`

And assignment only binds an identifier with the _new object (with the new address), but does not influence the (already previously) bound object_ as it would be in case of the reference:

```js
bar = {z: 1, q: 2};

alert([foo.x, foo.y]); // [100, 200] – nothing is changed
alert([bar.z, bar.q]); // [1, 2] – however bar now references the new object
```

I.e. now _foo_ and _bar_ have different values, different addresses:

```
foo value: addr(0xFF) => {x: 100, y: 200} (address 0xFF)
bar value: addr(0xFA) => {z: 1, q: 2} (address 0xFA)
```

Again, here all is related with that fact that variable values in case of object type _are addresses_, but _not object structures themselves_. Assignment of one variable to another — _copies_ its value-reference; thus both variables references the same place in memory. The next assignment of the new value (the new address) _unbinds_ a name from the old address and _binds_ it with the new. And it is the important difference from the _by reference_ strategy.
