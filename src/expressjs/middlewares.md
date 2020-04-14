# Middleware

Middleware functions are functions that have access to the request object `req`, the response object `res`, and the `next` middleware function in the application's request-response cycle. The next middleware function is commonly denoted by a variable named `next`.

Middleware functions can perform the following tasks:

* Execute any code.
* Make changes to the request and the response objects.
* End the request-response cycle.
* Call the next middleware in the stack.

```js
var myMiddleware = function(req, res, next) {
  // Do something with req and/or res
  next();
  // Calling this function invokes the next middleware function in the app.
  // The next() function is not a part of the Node.js or Express API, but is
  // the third argument that is passed to the middleware function.
  // The next() function could be named anything, but by convention it
  // is always named “next”.
};
```

> When writing your own middleware, don't forget to call the `next()` callback function. That means, If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.

> The request and response objects are the same for the subsequent middleware, so you can add properties to one middleware e.g `req.user = 'Abhay'` and access it in next middleware.

## How to use

`app.use(path, callback)` function.

_path_ - optional, string parameter

_callback_ - mandatory, function parameter

```js
// apply to all routes - Application level Middleware
app.use(function(req, res, next) {
  console.log('%s %s — %s', (new Date).toString(), req.method, req.url);
  return next();
});

//apply to /admin route only -  Route level Middleware
app.use('/admin', function(req, res, next) {
  console.log('%s %s — %s', (new Date).toString(), req.method, req.url);
  return next();
});
```

### monad

Choosing between middleware on certain condition.

```js
var myMiddleware = function(param) {
    if (param === 'A') {
      return function(req, res, next) { // <---Middleware A
        // Do A stuff
        return next();
      }
    } else {
      return function(req, res, next) { // The default middleware
        // Do default stuff
        return next();
      }
    }
```

> Middelwares are executed in orderly fashion, one by one from top to bottom. The order in which they are called using `app.use()`.

## return next()

To stop the further execution of code after `next()` and run next middleware then use `return` statement. Because just `next()` does not mean jump to next middleware immediately and forget about the line of code below it. The code still run after next statement. But if you put `return` then it say, jump to next middleware immediately and do not run code following it.

```js
/** Without return **/
var ping = function(req, res, next) {
  console.log('ping');
  next(); // does not stop the execution immediately
  console.log('HI'); // it runs and log `HI`

  // no more code then jump to next middleware
};

/** With return **/
var ping = function(req, res, next) {
  console.log('ping');
  return next(); // stop the execution immediately and jump to next middleware
  console.log('HI'); // does not run
};
```

## next('route')

To skip the rest of the middleware functions from a router middleware stack, call `next('route')` to pass control to the next route.

> `next('route')` will work only in middleware functions that were loaded by using the `app.METHOD()` or `router.METHOD()` functions.

```js
app.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next route with the same path '/user/:id'.
  // In this case it is handler which render 'special page'.
  if (req.params.id == 0) next('route');
  // otherwise pass the control to the next middleware function in this stack
  else next(); //
}, function (req, res, next) {
  // render a regular page
  res.render('regular');
});

// handler for the /user/:id path, which renders a special page
app.get('/user/:id', function (req, res, next) {
  res.render('special');
});
```

## next('parameter')

If you pass anything to the `next()` function (except the string `'route'`), Express regards the current request as being in error and will skip all remaining middlewares or handlers in the chain except for those middlewares that are set up to handle errors.

## Passing Arguments to middleware

```js
function requireRole(role) {
  return function(req, res, next) {
    if(req.session.user && req.session.user.role === role)
        next();
    else
       res.send(403);
  }
}

app.get("/foo/:id", requireRole("user"), foo.show);
app.post("/foo", requireRole("admin"), foo.create);
```

```js
/** Middleware File **/
var permission = function(roles) {
  return function(req, res, next) {
    var assignedRoles = Parse.User.current().get('roles');
    if (assignedRoles.indexOf('superAdmin') !== -1) {
      // if superAdmin role has assigned then no restrictions
      next();
    } else {
      // if role is present then the permission will be granted.
      if (!_.isEmpty(_.intersection(assignedRoles, roles))) {
        next();
      } else {
        res.renderView('accessDenied');
      }
    }
  };
};

module.exports = permission;

/** Route File **/
app.get("/foo/:id", permission("user"), foo.show);
app.post("/foo", permission("admin"), foo.create);
```
