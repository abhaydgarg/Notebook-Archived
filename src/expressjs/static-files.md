# Static Files

To serve static files such as images, CSS files, and JavaScript files, use the `express.static` built-in middleware function in Express.

```js
app.use(express.static(__dirname + '/public'));

// Now, you can load the files that are in the public directory.
// Note: Do not use 'public' in url, expressjs add it for you.

/*
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
*/
```

## Multiple Static Directories

call the `express.static` middleware function multiple times.

```js
app.use(express.static(__dirname + '/public')));
app.use(express.static(__dirname + '/files')));

// expressjs look for file in order in which the middleware are called. For eaxmple, it
//first look into 'public' directory, if not found then look into 'files' directory.
```

## Virtual Path

Hide Static directory

```js
app.use('/static', express.static('public'));

// Now, you can load the files that are in the public directory from the /static path prefix.

/**
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
**/
```
