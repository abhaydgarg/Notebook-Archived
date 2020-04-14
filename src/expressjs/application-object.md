# Application Object

## Properties

### app.locals

Set local variables for application. It persist throughout the life of the application, in contrast with `res.locals` properties that are valid only for the lifetime of the request.

**Use it to set local variables and helper function for template.**

```js
app.locals.title = 'My App';
app.locals.strftime = require('strftime');
```

**Local variables are available in middleware via** `req.app.locals`

### app.mountpath

Gives the parent route path to which sub app mounted.

```js
var express = require('express');
var versionOne = express();

admin.get('/', function (req, res) {
  console.log(versionOne.mountpath); // /v1
});

app.use('/v1', versionOne); // mount the sub app
```

## Event

### mount

The `mount` event is fired on a sub-app, when it is mounted on a parent app.

```js
// sub-app
var admin = express();

admin.on('mount', function (parent) {
  console.log('Admin Mounted');
  console.log(parent); // refers to the parent app's settings
});

app.use('/admin', admin);
```

## Methods

### Configuration

Set application configuration = `app.set('port', process.env.PORT || 3000);`

Get application configuration = `app.get('port')` - return `undefined` if setting is not present.

> Configuration set by `app.set('appName', 'Keep')` can be accessed in views templates with name `appName`.

**`app.enable( ) and app.disable( )`**

To set boolean type configuration such as:

```js
app.disable('etag');

/** OR **/

app.set('etag', false);
```

#### Custom settings

You can also set your own custom settings.

#### System Configuration

These settings are used by the framework to determine certain configurations. Most of them have default values.

#### env

Store the current environment mode.

1. development - **default**
2. test
3. stage
4. preview
5. production

```js
app.set('env', 'preview'); /* OR */ process.env.NODE_ENV = preview
```

#### view cache

Cache views template files.

development environment = view cache default value is false.

production environment = view cache default value is true.

#### view engine

The view engine setting holds the template file extension (e.g., 'ext' or 'jade') to utilize if the file extension is not passed to the `res.render()` function inside of the request handler.

**Default** = jade

```js
app.set('view engine', 'ejs');
```

#### views

absolute pathto a directory with templates.

```js
app.set('views', path.join(__dirname, 'views'));
```

#### jsonp callback name

If you're building an application (a REST API server) that serves requests coming from front-end clients hosted on different domains, you might encounter cross-domain limitations when making XHR/AJAX calls. In other words, browser requests are limited to the same domain (and port). The workaround is to use cross-origin resource sharing (CORS) headers on the server.

**Default** value = callback

#### json replacer and json spaces

When sending response back by `res.json()` , we can apply special parameters: **replacer** and **spaces**. These parameters are passed to all `JSON.stringify()` functions to alter the response text before sending to client(browser).

```js
//Define a replacer parameter as a function that omits the discount code from the object (we don’t want to expose this info). And the spaces parameter is set to 4 so that we can see JSON that is nicely formatted for humans instead of some jumbled code.
app.set('json replacer', function(key, value) {
  if (key === 'discount')
    return undefined;
  else
    return value;
});
app.set('json spaces', 4);
```

#### case sensitive routing

Default = false

If true then, `/users` and `/Users` won't be the same.

#### strict routing

default = false

Deals with cases of trailing slashes in URLs. With strict routing enabled, such as `app.set('strict routing', true');`, the paths will be treated differently; for example, `/users` and `/users/` will be completely separate routes.

### app.path()

Return canonical path of app or sub-app.

```js
var app = express()
  , blog = express()
  , blogAdmin = express();

app.use('/blog', blog);
blog.use('/admin', blogAdmin);

console.log(app.path()); // ''
console.log(blog.path()); // '/blog'
console.log(blogAdmin.path()); // '/blog/admin'
```

> The behavior of this method can become very complicated in complex cases of mounted apps: it is usually better to use `req.baseUrl` to get the canonical path of the app.

### app.render(view, [locals], callback)

Returns the rendered HTML of a view. It is like `res.render()`, except it cannot send the rendered HTML to the client/browser on its own. That means you get the rendered HTML/final product but it is not going to send to browser. To send the HTML content to browser, we use `res.render()`.

```js
app.render('email', { name: 'Tobi' }, function(err, html){
  // ...
});
```

### app.use([path,] function [, function...])

Mounts the specified middleware function or functions at the specified path.

> If path is not specified, it defaults to “/”

```js
// A route will match any path that follows its path
// immediately with a “/”. For example: app.use('/apple', ...) will match
// “/apple”, “/apple/images”, “/apple/images/news”, and so on.

app.use('/admin', function(req, res, next) {
  // GET 'http://www.example.com/admin/new'
  // '/admin/new' - Note: req.originalUrl in a middleware function
  // is a combination of req.baseUrl and req.path
  console.log(req.originalUrl);
  console.log(req.baseUrl); // '/admin'
  console.log(req.path); // '/new'
  next();
});

// Since path defaults to “/”, middleware mounted without
// a path will be executed for every request to the app.
// This middleware will be executed for every request to the app
app.use(function (req, res, next) {
  console.log('Time: %d', Date.now());
  next();
});
```

**Important:**

 Middleware functions are executed sequentially, therefore the order of middleware inclusion is important.

```js
// this middleware will not allow the request to go beyond it
app.use(function(req, res, next) {
  res.send('Hello World');
});

// requests will never reach this route
app.get('/', function (req, res) {
  res.send('Welcome');
});
```

#### Different Ways to Define Path

Used both in `app.use()` and `app.VERB()` route method.

##### 1. Path

```js
// This will match paths starting with `/abcd`:
app.use('/abcd', function (req, res, next) {
  next();
});
```

##### 2. Pattern

```js
// This will match paths starting with `/abcd` and `/abd`:
app.use('/abc?d', function (req, res, next) {
  next();
});

// This will match paths starting with `/abcd`, `/abbcd`, `/abbbbbcd`, and so on:
app.use('/ab+cd', function (req, res, next) {
  next();
});

// This will match paths starting with `/abcd`, `/abxcd`, `/abFOOcd`, `/abbArcd`, and so on:
app.use('/ab\*cd', function (req, res, next) {
  next();
});
```

##### 3. Regular Expression

```js
// This will match paths starting with `/abc` and `/xyz`:
app.use(/\/abc|\/xyz/, function (req, res, next) {
  next();
});
```

##### 4. Array

```js
// This will match paths starting with `/abcd`, `/xyza`, `/lmn`, and `/pqr`:
app.use(['/abcd', '/xyza', /\/lmn|\/pqr/], function (req, res, next) {
  next();
});
```
