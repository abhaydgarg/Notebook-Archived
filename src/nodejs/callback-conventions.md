# Callback conventions

## The continuation-passing style

In JavaScript, a callback is a function that is passed as an argument to another function and is invoked with the result when the operation completes. In functional programming, this way of propagating the result is called continuation-passing style, for brevity, **CPS**. It is a general concept, and it is not always associated with asynchronous operations. In fact, it simply indicates that **a result is propagated by passing it to another function (the callback), instead of directly returning it to the caller.**

## Callbacks come last

In Node.js, if a function accepts in input a callback, this has to be passed as the last argument. Let's take the following Node.js core API as an example:

```js
fs.readFile(filename, [options], callback)
```

As you can see from the signature of the preceding function, the callback is always put in last position, even in the presence of optional arguments. The motivation for this convention is that the function call is more readable in case the callback is defined in place.

## Error comes first

If the operation succeeds without errors, the first argument will be `null`. The following code shows you how to define a callback complying with this convention:

```js
fs.readFile('foo.txt', 'utf8', function(err, data) {
  if(err)
    handleError(err);
  else
    processData(data);
});
```

!!! note
    Another important convention to take into account is that the error must always be of **type Error.** This means that simple strings or numbers should never be passed as error objects.

## Propagating errors

Propagating errors in synchronous, direct style functions is done with the well-known `throw` command, which causes the error to jump up in the call stack until it's caught.

In asynchronous CPS however, proper error propagation is done by simply passing the error to the next callback in the CPS chain. The typical pattern looks as follows:

```js
var fs = require('fs');

function readJSON(filename, callback) {
  fs.readFile(filename, 'utf8', function(err, data) {
    var parsed;
    if (err)
      //propagate the error and exit the current function
      return callback(err);
    try {
      //parse the file contents parsed = JSON.parse(data);
    } catch (err) {
      //catch parsing errors
      return callback(err);
    }
    //no errors, propagate just the data callback(null, parsed);
  });
};
```
