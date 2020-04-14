# Points to note

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
