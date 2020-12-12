# Express.js

## Middleware

### Choosing between middleware on certain condition

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

## EJS

```ejs
// execute js but behave as unbuffered code means code is executed but does
// not print or send to output buffer. Used for conditional statement etc.
<% code %>

// print escaped HTML. Buffered code means code is executed and print the result
<%= code %>

//print unescaped HTML. Buffered code.
<%- code %>

//Filters. capitalize the name.
<%=: name | capitalize %>

// include partial
<% include header.ejs %>

//Custom delimiters
var ejs = require('ejs');
ejs.open = '{{';
ejs.close = '}}';

<h1>{{= title }}</h1>
```

## Use `return` to stop function execution

```js
/* Not recommended */
module.exports.save = function(req, res, next) {
    // send final result to browser along with headers
    res.send('hi');
    // log to console but generate already sent header warning
    console.log('here');
};

// ## Solutions

/* 1. Must be last statement in function */
module.exports.save = function(req, res, next) {
  console.log('here');
  res.send('hi'); // must be in last
};

/* 2. use return */
module.exports.save = function(req, res, next) {
    // send final result to browser along with headers
    return res.send('hi');
    // do not log to console
    console.log('here');
};
```

Even when using callback, you can use `return` to stop

```js
/* 1 way */
function(value, cb) {
  if(value) {
    cb(null, value + 1);
  } else {
    cb('error')
  }
}

/* 2 way */
function(value, cb) {
  if(!value) {
    cb('error');
    return;
  }
  cb(null, value + 1);
}
```
