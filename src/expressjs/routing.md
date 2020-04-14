# Routing

## Supported HTTP methods

### app.VERB()

`get`, `post`, `put`, `head`, `delete`, `options`, `trace`, `copy`, `lock`, `mkcol`, etc.

### Special method - app.all()

This method used to load middleware for specific path or all path irrespective of HTTP method.

```js
// the handler will be executed for requests to “/secret”
// whether you are using GET, POST, PUT, DELETE, or any
// other HTTP request method.

app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...');
  next(); // pass control to the next handler
});

// the handler will be executed for all requests whether
// you are using GET, POST, PUT, DELETE, or any other
// HTTP request method.

app.all('*', function (req, res, next) {
  console.log('Accessing the secret section ...');
  next(); // pass control to the next handler
});
```

## Route path

Route paths can be strings, string patterns, or regular expressions.

```js
app.get('/random.text', function (req, res) {
  res.send('random.text');
});

// This route path will match acd and abcd.
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

// This route path will match abcd, abxcd, abRANDOMcd, ab123cd, and so on.
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// This route path will match anything with an “a” in the route name.
app.get(/a/, function(req, res) {
  res.send('/a/');
});
```

The same path can be provided for different methods.

```js
// get method
app.get('/users', function (req, res) {
});

//post method
app.post('/users', function (req, res) {
});
```

> Query strings are not part of the route path. If provided during path declaration then it is not taken into consideration.

## Path parameter

`:parameterName`

```js
/user/:id
```

## app.param()

Add callback triggers to route parameters. To summarize, by defining `app.param` once, its logic will be triggered for every route that has the matching URL parameter name.

```js
// :username is the parameter
app.param('username', function(request, response, next, username) {
  // ... Perform database query and
  // ... Store the user object from the database in the req object
  req.user = user;
  return next();
});

// No need of extra lines of code since we have req.user
// object populated by the app.param():
app.get('/users/:username', function(request, response, next) {
  //... Do something with req.user
  return res.render(req.user);
});
```

## Route handler

Different way to provide route handler and middlewares.

```js
// 1. Single handler without middleware
app.get('/example/a', function (req, res) {
  res.send('Hello from A!');
});

// 2. multiple middlewares and single handler - as chain mechanism
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});

// 3. multiple middlewares and single handler - as array
var cb0 = function (req, res, next) {
  console.log('CB0'); // act as middleware
  next();
}
var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}
var cb2 = function (req, res) {
  res.send('Hello from C!');
}
app.get('/example/c', [cb0, cb1, cb2]);

// 4. multiple middlewares as array and single handler
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}
var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}
app.get('/example/d', [cb0, cb1], function (req, res) {
   res.send('Hello from D!');
});
```

## app.route()

Use it to create chainable route handlers having same path but different request method.

```js
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```

## Routers - 4.x

**Router class/object.**

Starting with Express.js v4.x, using the new Router class is the recommended way to define routes.

Use the `express.Router` class to create modular, mountable route handlers. A `Router` instance is a complete middleware and routing system; for this reason, it is often referred to as a "mini-app". Once you've created a router object, you can add middleware and HTTP method routes (such as `get`, `put`, `post`, and so on) to it just like an application.

```js
// The following example creates a router as a module,
// loads a middleware function in it, defines some routes,
// and mounts the router module on a path in the main app.

/** file = birds.js **/

var express = require('express');
var router = express.Router(options); // create Router instance

// where options are:
// caseSensitive (default = false) = if it’s set to false, then /Users is the same as /users.
// strict (default = false) = if it’s set to false, then /users is the same as /users/.// middleware that is specific to this router

router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// define the home page route
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// define the about route
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;

/** Then load to file app.js */
var birds = require('./birds');

app.use('/birds', birds);

// The app will now be able to handle requests to `/birds`
// and `/birds/about`, as well as call the timeLog
// middleware function that is specific to the route.
```

**Router class is very well suited for writing modular routing.**

 See example below:

```js
var express = require('express');
var router = express.Router();

router.get('/users/:username',  function(request, response) {
    return response.render('user', request.user);
  }
);
router.get('/admin/:username',  function(request, response) {
    return response.render('admin', request.user);
  }
);

app.use('/v1', router);

// As you can see, router.get() methods include no mention
// of v4 because they become `/v1/users/:username` and
// `/v1/admin/:username`. If you have a large application
// with many versions of API and routes (v1, v2, etc.),
// then it’s better to use the Router class/object to
// organize the code of these routes. You create a  Router
//object and mount it on a path, such as /v1 and /v2.
```

> router.use() is same as app.use()
> router.param() is same as app.param()
> router.all() is same as app.all()
> router.route() is same as app.route()
> router.VERB() is same as app.VERB()

```js
/** Example of router.use() **/
var express = require('express');
var app = express();
var router = express.Router();

// simple logger for this router's requests
// all requests to this router will first hit this middleware
router.use(function(req, res, next) {
  console.log('%s %s %s', req.method, req.url, req.path);
  next();
});

// this will only be invoked if the path starts with /bar from the mount point
router.use('/bar', function(req, res, next) {
  // ... maybe some additional /bar logging ...
  next();
});

app.use('/foo', router);
```

**Note:** middleware added via one router may run for other routers if its routes match.

```js
var authRouter = express.Router();
var openRouter = express.Router();

authRouter.use(require('./authenticate').basic(usersdb));

authRouter.get('/:user_id/edit', function(req, res, next) {
  // ... Edit user UI ...
});
openRouter.get('/', function(req, res, next) {
  // ... List users ...
})
openRouter.get('/:user_id', function(req, res, next) {
  // ... View user ...
})

app.use('/users', authRouter);
app.use('/users', openRouter);

// Even though the authentication middleware was added via
// the authRouter it will run on the routes defined by the
// openRouter as well since both routers were mounted on
// /users. To avoid this behavior, use different paths for
// each router.
```

## Router Class/Object and Sub-App

It is used to create sub-app which can either inherit from main app or has own settings different from main app. Routers are just like sub-application except that they don't create a completely new app and inherits their settings from the main application. It's truely just a middleware.

### ExpressJS 4.x

```js
var express = require('express');

// Main application.
var app = express();
app.set('view engine', 'html');
app.set('views', require('path').join(__dirname, 'my-views'));
app.engine('html', require('ejs').renderFile);

// Website application.
var website = new express.Router();
website.get('/', function (req, res) {
  res.render('home');
});

// API application.
var api = new express.Router();
api.get('/articles', function (req, res) {
  res.send([{ title: 'hello' }]);
});

// Mouting applications.
app.use('/', website);
app.use('/api', api);

// Start listening.
app.listen(3000);
```

The main difference between sub-applications and routers are the settings inheritance. Express 4 introduce the router, so you can now modularize your applications without complicate things with problems of settings inheritance. You can create a sub-application in express if you want and especially if you need two completely separated applications without settings inheritance.

#### Benefits

keeping code for various routes completely separate. For example. `/admin` you could have different set of routes which uses views and for `/api`, you could have different set of routes which does not use views.
