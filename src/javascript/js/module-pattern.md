# Use Module Pattern to Organise Code

- Use Anonymous Closures

```js
(function () {
  // ... all vars and functions are in this scope only

}());
```

- Use Global Import

```js
(function ($, window) {
  // now have access to globals jQuery (as $) and window in this code
  // localize commonly accessed global variables, such as window, document, and jQuery
}(jQuery, window));
```

- Ways to write it

```js
// ### Anonymous Object Literal return ###

var Module = (function () {

  var privateMethod = function () {};

  return {
    publicMethodOne: function () {
      // I can call `privateMethod()` you know...
    },
    publicMethodtwo: function () {

    },
    publicMethodThree: function () {

    }
  };

})();

// ### Locally scoped Object Literal ###

var Module = (function () {

  // locally scoped Object
  var myObject = {};

  // declared with `var`, must be "private"
  var privateMethod = function () {};

  myObject.someMethod = function () {
    // take it away Mr. Public Method
  };

  return myObject;

})();

// ### Revealing Module Pattern ###

var Module = (function () {

  var privateMethod = function () {
    // private
  };

  var someMethod = function () {
    // public
  };

  var anotherMethod = function () {
    // public
  };

  return {
    someMethod: someMethod,
    anotherMethod: anotherMethod
  };

})();
```

- Augment Modules to extend it with additional functionality

Let's assume the following code:

```js
var Module = (function () {

  var privateMethod = function () {
    // private
  };

  var someMethod = function () {
    // public
  };

  var anotherMethod = function () {
    // public
  };

  return {
    someMethod: someMethod,
    anotherMethod: anotherMethod
  };

})();
```

So far our Object for Module would look like:

```js
Object {someMethod: function, anotherMethod: function}
```

But what if I want to add our Module extension, so it ends up with another public method, maybe like this:

```js
Object {someMethod: function, anotherMethod: function, extension: function}
```

Let's create an aptly named ModuleTwo, and pass in our Module namespace, which gives us access to our Object to extend. We could then create another method inside this module, have all the benefits of private scoping/functionality and then return our extension method.

```js
var Module = (function (Module) {

    Module.extension = function () {
        // another method!
    };

    return Module;

})(Module || {});
```

Module || {} into my second ModuleTwo, this is in case Module is undefined - we don't want to cause errors. Module gets passed into ModuleTwo, an extension method is added and then returned again. Module now has a third property:

```js
console.log(Module);
// Object {someMethod: function, anotherMethod: function, extension: function}
```
