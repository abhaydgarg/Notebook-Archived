# Globals

`__filename` and `__dirname`

These variables are available in each file and give you the full path to the file and directory for the current module.

## process

you use the process object to access the command line arguments. The arguments are available as the `process.argv` member property, which is an array.

`process.nextTick` is a simple function that takes a callback function. It is used to put the callback into the next cycle of the Node.js event loop. It is designed to be highly efficient, and it is used by a number of Node.js core libraries.

```js
process.nextTick(function() {
  console.log('next tick');
});
console.log('immediate');

// output
immediate
next tick
```

**buffer** - Binary Data

Pure JavaScript does not handle straight binary data very well, though JavaScript is Unicode friendly. When dealing with TCP streams and reading and writing to the filesystem, it is necessary to deal with purely binary streams of data. A buffer is a region of a physical memory storage used to temporarily store data while it is being moved from one place to another. In node each buffer corresponds to some raw memory allocated outside V8. A buffer acts like an array of integers, but cannot be resized. The Buffer class is a global. It deals with binary data directly and can be constructed in a variety of ways.

```js
var buffer = new Buffer(n); //n = size of buffer
```

## global

The variable `global` is our handle to the global namespace in Node.js. If you are familiar with front-end JavaScript development, this is somewhat similar to the `window` object. All the true globals we have seen (console, setTimeout and process) are members of the global variable. You can even add members to the global variable to make it available everywhere.

```js
// foo.js
global.something = 123;

// app.js
require('./foo');
console.log(something); // 123
```
