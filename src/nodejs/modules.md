# Modules

## What is `./`

`./some_file` means run `some_file` in the current directory. `../some_file` means run some file in the parent directory.

Even if the file in same directory, you have to use `./` because it tells node that it a file-based module and not a core or `node_modules` module.

Node treats path relative; if `./` or `../` or `/` is present otherwise not.

## Extension

Do not use `.js` extension when requiring a module.

## How module is scanned

### File based _[relative path]_

If the module name passed into the require function is prefixed with `./` or `../`, then it is assumed to be a file-based module, for example `require('./bar), require('../bar/bar')` and load the module from path specified.

> No scanning is required in case of file module.

### Core modules and package node_modules _[non-relative path]_

**[1]** If moduleName is not prefixed with "/" or "./", the algorithm will first try to search within the core Node.js modules. If found then node stops right there and does not bother to look into `node_modules`.

**[2]** If no core module is found matching module name, then the search continues by looking for a matching module into the first `node_modules` directory that is found navigating up in the directory structure starting from the requiring module. The algorithm continues to search for a match by looking into the next `node_modules` directory up in the directory tree, until it reaches the root of the filesystem.

For example, if the file is at `'/home/ry/projects/foo.js'` called `require('bar')`, then node would look in the following locations, in this order:

```
[1]: node_modules of current dir.
/home/ry/projects/node_modules/bar.js

[2]: node_modules of one level up dir.
/home/ry/node_modules/bar.js

[3]: node_modules of two level up dir.
/home/node_modules/bar.js

[4]: node_modules of project root dir - ends here at the project root.
/node_modules/bar.js
```

## require()

**Blocking:** The require function blocks further code execution until the module has been loaded. This means that the code following the require call is not executed until the module has been loaded and executed.

**Caching:** Modules are cached the first time they are loaded, which means that every call to `require('myModule')` returns exactly the same module.

**Shared State:** Having some mechanism to share state between modules is useful in various contexts. Since modules are cached, every module that require `foo.js` will get the same (mutable) object if we return an object `foo` from a module `foo.js`.

> require() inside function is also cached and put in global scope of nodejs. So calling require() inside function place module in global cache and when you access the module again using cache even top of file you will get cached module.

```js
// foo.js
module.exports = {
  something: 123
};

// app.js
var foo = require('./foo');
console.log('initial something:', foo.something); // 123
// Now modify something:
foo.something = 456;

// bar.js
var foo = require('./foo');
console.log('in another module:', foo.something); // 456
```

### How to avoid shared state - Object Factory

```js
// foo.js

// whenever you call function it returns new reference of object because of object literal
module.exports = function() {
  return {
    something: 123
  };
};

// app.js
var foo = require('./foo');
// create a new object
var obj = foo();
// use it
console.log(obj.something); // 123

//Note: you can even do this in one step require('./foo')();
```

## Module caching caveat

> **Question:** Are Node.js modules singletons?

Node.js will cache the module after the first invocation of require(), making sure to not execute it again at any subsequent invocation, returning instead the cached instance.

**But there is a caveat;** the module is cached using its **full path as lookup key**, therefore
it is guaranteed to be a shared (singeleton) only within the current package.

Let's understand it by an example:

```
module.exports = new Database('my-app-db');
```

By simply exporting a new instance of our database, we can already assume that within the current package (which can easily be the entire code of our application), we are going to have only one instance of the `db` module.

Now as we know that each package might have its own set of private dependencies inside its `node_modules` directory, which might result in multiple instances of the same package and therefore of the same module, with the result that our shared module might not be singleton or shared anymore.

```json
{
  "name": "mydb",
  "main": "db.js"
}
```

```js
app/
-- node_modules
  |-- packageA
  |   -- node_modules
  |      -- mydb
   -- packageB
      -- node_modules
         -- mydb
```

Both `packageA` and `packageB` have a dependency on the `mydb` package; in turn, the `app` package, which is our main application, depends on `packageA` and `packageB`. The scenario we just described, will break the assumption about the uniqueness of the database instance; in fact, both `packageA` and `packageB` will load the database instance using a command such as the following:

```js
var db = require('mydb');
```

However, `packageA` and `packageB` will actually load two different instances of `mydb` module, because the `mydb` module will resolve to a two different directory depending on the package it is required from.
