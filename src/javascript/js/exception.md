# Exception

## Syntax error

Syntax error, if program strucutre is not complied to the rules then javasript stop executing. Script “dies” (immediately stops) in case of an error, printing it to console. For example:

```js
function fn() {
  // Syntax error.
  new new;
  return null;
}

fn();
```

There is not point executing the code further if there is a syntax error.

> **Syntax errors are not cachable using `try/catch`**. Before `try/catch` to work you must have code which is syntactically correct.

> For `try/catch` to work, the code must be runnable. See the example above.

In other words, it should be valid JavaScript. It won’t work if the code is syntactically wrong. So, `try/catch` can only handle errors that occur in the valid code. Such errors are called “runtime errors” or, sometimes, “exceptions”.

## try/catch

`try/catch` that allows to “catch” errors and, instead of dying, do something more reasonable.

```js
console.log(1);

// Script stops here.
console.log(a);

console.log(2);
```

```
# Output
1
Uncaught ReferenceError: a is not defined
```

```js
console.log(1);

try {
  console.log(a);
} catch (e) {
  // Catch error here and continue execution.
  console.error(e.message);
}

console.log(2);
```

```
# Output

1
a is not defined
2
```

### JSON.parse

`JSON.parse()` evaluate the javasript string and return javasript code out of it. So sometimes if string is not a valid json string then javasript throws a syntax error, which means your code will die. So instead checking if the string is a valid json, you can wrap the `JSON.parse` in `try/catch` and let not the script die unexpectingly.

```js
console.log(1);

// Invalid json
let json = '["test" : 123]';
try {
  JSON.parse(json);
} catch (e) {
  console.log(e.message);
}

console.log(2);
```

```
# Output

1
Unexpected token : in JSON at position 8
2
```

### Work synchronously

If an exception happens in “scheduled” code, like in `setTimeout`, then `try..catch` won’t catch it:

```js
try {
  setTimeout(function() {
    noSuchVariable; // script will die here
  }, 1000);
} catch (e) {
  alert( "won't work" );
}
```

That’s because  `try..catch`  actually wraps the  `setTimeout`  call that schedules the function. But the function itself is executed later, when the engine has already left the  `try..catch`  construct.

To catch an exception inside a scheduled function,  `try..catch`  must be inside that function:

```js
setTimeout(function() {
  try {
    noSuchVariable; // try..catch handles the error!
  } catch {
    alert( "error is caught here!" );
  }
}, 1000);
```

## throw

> `throw` stops the script execution if not catched.

## Without Error object

> Any expression after `throw`.

```js
// generates an exception with a string value
throw 'Error2';
// generates an exception with the value 42
throw 42;
// generates an exception with the value true
throw true;
// generates an exception with the value object
throw {extra: 'info', debug: 'info'};
```

```js
try {
  throw 'Index is missing';
} catch (e) {
  // `e` here is a string.
  console.error(e); // Index is missing.
}
```

### Throwing object

```js
let obj = { extra: 'info', debug: 'message' };
try {
  // Can be any expression, even object.
  throw obj;
} catch (e) {
  // e here is a object
  console.log(e); // { extra: 'info', debug: 'message' }
}
```

Technically, we can use anything as an error object. That may be even a primitive, like a number or a string, but it’s better to use objects, preferably with `name` and `message` properties (to stay somewhat compatible with built-in errors).

## With Error object

> Error arg is of type string.

```js
// generates an error object with the message of Required
throw new Error('Required');
```

```js
try {
  throw new Error('Index is missing');
} catch (e) {
  // `e` here is a object.
  console.error(e.name); // Error.
  console.error(e.message); // Index is missing.
}
```

### Throwing object

```js
let obj = { extra: 'info', debug: 'message' };
try {
  // linting error.
  // Can only be string arg.
  throw new Error(obj);
} catch (e) {
  // e.message is a string.
  // object is coerced to string.
  console.log(e.message); // [object Object]
}
```

**Solution** - `JSON.stringify`

```js
let obj = { extra: 'info', debug: 'message' };
try {
  throw new Error(JSON.stringify(obj));
} catch (e) {
  console.log(JSON.parse(e.message));
}
```

## Rethrow

**Catch should only process errors that it knows and “rethrow” all others.**

The “rethrowing” technique can be explained in more detail as:

1. Catch gets all errors.
2. In  `catch(err) {...}` block we analyze the error object `err`.
3. If we don’t know how to handle it, then we do `throw err`.

In the code below, we use rethrowing so that `catch` only handles `SyntaxError`:

```js
let json = '{ "age": 30 }'; // incomplete data
try {

  let user = JSON.parse(json);

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }

  blabla(); // unexpected error

  alert( user.name );

} catch(e) {

  if (e.name == "SyntaxError") {
    alert( "JSON Error: " + e.message );
  } else {
    throw e; // rethrow (*)
  }

}
```

The error throwing on line `(*)` from inside `catch` block “falls out” of `try..catch` and can be either caught by an outer `try/catch` construct (if it exists), or it kills the script.

So the `catch`  block actually handles only errors that it knows how to deal with and “skips” all others.

The example below demonstrates how such errors can be caught by one more level of `try/catch`:

```js
function readData() {
  let json = '{ "age": 30 }';

  try {
    // ...
    blabla(); // error!
  } catch (e) {
    // ...
    if (e.name != 'SyntaxError') {
      throw e; // rethrow (don't know how to deal with it)
    }
  }
}

try {
  readData();
} catch (e) {
  alert( "External catch got: " + e ); // caught it!
}
```

Here `readData` only knows how to handle `SyntaxError`, while the outer `try/catch` knows how to handle everything.

## try/catch/finally

If it exists, it runs in all cases:

- after `try`, if there were no errors,
- after `catch`, if there were errors.

```js
try {
   // ... try to execute the code ...
} catch(e) {
   .// .. handle errors ...
} finally {
   // ... execute always ...
}
```

### finally and return

The `finally` clause works for _any_ exit from `try..catch`. That includes an explicit `return`.

In the example below, there’s a `return` in `try`. In this case, `finally` is executed just before the control returns to the outer code.

```js
function func() {

  try {
    return 1;
  } catch (e) {
    /* ... */
  } finally {
    alert( 'finally' );
  }
}

// first works alert from finally, and then this one
alert( func() );
```

## JavaScript built in Error constructors

JavaScript has many built-in constructors for standard errors: `Error`, `SyntaxError`, `ReferenceError`, `TypeError`and others. We can use them to create error objects as well.

> **name** property tells you about the `type` of error.

```js
// e.name = Error
let error = new Error(message);
// e.name = SyntaxError
let error = new SyntaxError(message);
// e.name = ReferenceError
let error = new ReferenceError(message);
```

## Points to note

### [1]

`throw` statement stop the execution of whole script if uncaught. Even throw in a function, it stops function execution as well as script execution as whole.

```js
function fn() {
  throw new Error('eeee...');
}
fn();
console.log(1); // won't run.
```

```
Uncaught Error: eeee...
  at fn
```

### [2]

If you throw and error using `throw` statement in a function without `try/catch` then the error thrown is captured by parent function's `try/catch`. If parent function does not have `try/catch` then javascript look for `try/catch` in next parent in function stack hierarchy. If error is not handled by any `try/catch` then script stops by uncaught exception message.

```js
function child() {
  throw new Error('eeee...');
}

function parent() {
  child();
}

function ancestor() {
  parent();
}

ancestor();
console.log('Hello'); // Won't run.
```

```
Uncaught Error: eeee...
  at child
  at parent
  at ancestor
```

```js
function child() {
  throw new Error('eeee...');
}

function parent() {
  child();
}

function ancestor() {
  try {
    parent();
  } catch (e) {
    console.log(e.message);
  }
}

ancestor();
console.log('Hello'); // Will run.
```

```
eeee...
Hello
```

### [3]

If you throw an error inside `try` block then that error must be handled by the its own `catch` block. If you do not do so then the error go unnoticed because it will also not be captured by parent's `catch` block. So you must handled any error inside `try` block in its own `catch` block.

```js
function child() {
  try {
    throw new Error('eeee...');
  } catch (e) {
    // not handled here.
  }
}

function parent() {
  try {
    child(); // throw an error.
  } catch (e) {
    // Won't get the error `eeee...` here.
    // Go unnoticed.
    console.log(e.message);
  }
}
parent();
console.log(1);
```

```
1
```

**Solution** - Check point [4] below.

### [4]

If you want to propagate or want to handle the error thrown inside the `try` block by its parent `catch` block then you first have to capture it in its own `catch` block and then `rethrow` it.

```js
function child() {
  try {
    throw new Error('eeee...');
  } catch (e) {
    // rethrow
    throw e;
  }
}

function parent() {
  try {
    child(); // throw an error.
  } catch (e) {
    console.log(e.message);
  }
}
parent();
console.log(1);
```

```
eeee...
1
```

### [5]

If error object that you get in `catch` block is generated by `new Error()` then while rethrowing it again as it is in catch block, do not use `new Error()` statement again. Simply throw it using statement `throw`. There is no need to wrap it again using `new`.

```js
function fn() {
  try {
    throw new Error('eeee...');
  } catch (e) {
    // rethrow
    // e is already is of type Error.
    // throw new Error(e); // Not recommended.
    throw e; // Recommended.
  }
}
```

### [6]

The usefulness of `try/catch` is that; if any statement or line of code throw an exception in `try` block then you can handle the error gracefully in `catch` block and prevent an application from sudden death. From `catch` you can return some other data back to calling function.

```js
function getUser () {
  try {
    const user = fetchUser();
    return user;
  } catch(e) {
    // assume fetchUser() call is failed
    // then in catch block you can return
    // empty object to parent function and
    // log an error.
    console.error(e);
    return {};
  }
}

function processUser() {
  // Either you get User object
  // or empty object when failed
  // and script won't stop on error.
  const user = getUser();
}

processUser();
```

### [7]

Avoid the need to check or validate object key presence using multiple `if` statements.

```js
function getMetadataFromStorage (id) {
  if (sessionStorage !== undefined) {
    // null is returned if key does not exist
    let metadata = sessionStorage.getItem(`${hackerNewsXStoryKey}${id}`);
    if (metadata !== null) {
      try {
        // Try/catch block because local storage
        // can manipulated by other sources and
        // you may not get what you are expecting
        // so if JSON parsing is failed, storage is not
        // object, if storage is object then metadata key
        // is missing and metadata is present but it is not object.
        // So instead deeply check the type and key of storage variable
        // using multiple if statements, it is better to use try/catch
        // which catch any type of error.
        return JSON.parse(metadata);
      } catch (e) {
        Util.consoleWarn(`Cannot get metadata from storage [${id}]`);
      }
    }
  }
}
```
