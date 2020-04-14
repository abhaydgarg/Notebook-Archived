# Types of scope

## Lexical (static) scope

The word static relates to ability to determine the scope of a variable during the parsing stage of a program. That is, before the execution the program, by just looking on the code we can say , in which scope a variable will be resolved.

```js
var x = 10;
var y = 20;

function foo() {
  console.log(x, y);
}

foo(); // 10, 20

function bar() {
  var y = 30;
  console.log(x, y); // 10, 30
  foo(); // 10, 20
}

bar();
```

In the example above, variable `x` lexically defined in the global scope -- that means at runtime it is resolved also in the global scope, i.e. to value 10.

In case of the `y`, we have two definitions. But always the nearest lexical scope containing the variable is considered. The own scope has the highest priority and is considered the first. Therefore, in case of the `bar()` function, `y` variable is resolved as 30.

However, the same `y` is resolved as 20 in case of `foo()` function -- even if it is called inside the `bar()` function, which contains another `y`. The resolution of an variable is an independent from the environment of a caller (in this case `bar()` is a caller of `foo()`, and `foo()` is a callee). And again, this is because at the moment of `foo()` function definition, the nearest lexical context for the `y` variable was the global context.

Today static scope is used in many languages: C, Java, ECMAScript, Python, Ruby, Lua, etc.

## Dynamic scope

In contrast with the static scope, in dynamic scope we cannot determine at parsing stage, to which value a variable will be resolved. The resolution of the variable depending on the context from which the function is called.

The variable is resolved in the dynamically formed global stack of variables. Every met variable declaration just puts the variable onto the stack.

For example, consider a similar case as from above, but with using the dynamic scope. We use pascal-like pseudo-code syntax:

```pascal
// *pseudo* code - with dynamic scope

y = 20;

procedure foo()
  print(y)
end


// on the stack of the "y" name currently only one value 20
// {y: [20]}

foo() // 20, OK

procedure bar()

  y = 30

  // and now on the stack there, are two "y" values: {y: [20, 30]};
  // the first found (from the top) is taken
  // therefore:

  foo() // 30!, not 20

end

bar()
```

We see that the environment of a caller affects on the variables resolution. Since a function (a callee) may be called from many different locations and with different states, it's hard to determine the exact environment statically, at parsing stage. That's why this type of scope is called as dynamic.

That is, a dynamically-scoped variable is resolved in the environment of execution, rather than the environment of definition as we have in the static scope.

Today, most of the modern languages do not use dynamic scope. However, in some languages, and notably in Perl (or some variations of Lisp), a programmer may choose how to define a variable -- with static or dynamic scope.
