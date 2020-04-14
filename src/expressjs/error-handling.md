# Error handling

Define error-handling middleware functions in the same way as other middleware functions, except error-handling functions have four arguments instead of three: `(err, req, res, next)`.

```js
//You define error-handling middleware last, after other app.use() and routes calls
var bodyParser = require('body-parser');
var methodOverride = require('method-override');

app.use(bodyParser());
app.use(methodOverride());
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

## Multiple Error Handlers

```js
function logErrors(err, req, res, next) {
  console.error(err.stack);
  next(err);
}

function clientErrorHandler(err, req, res, next) {
  if (req.xhr) {
    res.status(500).send({ error: 'Something failed!' }); // stop here if xhr request
  } else {
    next(err);
  }
}
// The “catch-all” errorHandler
function errorHandler(err, req, res, next) {
  res.status(500);
  res.render('error', { error: err });
}

var bodyParser = require('body-parser');
var methodOverride = require('method-override');

app.use(bodyParser());
app.use(methodOverride());
app.use(logErrors); // write error to stderr
app.use(clientErrorHandler); // send error in json for xhr request
app.use(errorHandler); // catch all errors if not handled by previous handlers
```
