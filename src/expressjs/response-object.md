# Response Object

## Properties

### res.app

Holds a reference to the instance of the Express application

### res.locals

An object that contains response local variables scoped to the request, and therefore available only to the view(s) rendered during that request / response cycle (if any).

```js
// This property is useful for exposing request-level information
// such as the request path name,
// authenticated user, user settings, and so on.

app.use(function(req, res, next){
  res.locals.user = req.user;
  res.locals.authenticated = ! req.user.anonymous;
  next();
});
```

## Methods

### res.end()

Ends the response process. This method actually comes from Node core, specifically the `response.end()` method of `http.ServerResponse`.

```js
// Use to quickly end the response without any data.
// If you need to respond with data, instead
// use methods such as res.send() and res.json().

res.end();
res.status(404).end();
```

### res.json([body])

Sends a JSON response.

```js
res.json(null);
// object converted in string by JSON.stringify() before sending.
res.json({ user: 'tobi' });
res.status(500).json({ error: 'message' });
```

### res.send([body])

Sends the HTTP response.

The `body` parameter can be a `Buffer` object, a `String`, an object, or an `Array`. For example:

```js
res.send(new Buffer('whoop'));
// converted in string by JSON.stringify() before sending
res.send({ some: 'json' });
// converted in string by JSON.stringify() before sending
res.send([1,2,3]);
res.send('<p>some html</p>');
res.status(404).send('Sorry, we cannot find that!');
res.status(500).send({ error: 'something blew up' });

// When the parameter is a Buffer object, the method
// sets the Content-Type response header field to
// “application/octet-stream”, unless previously defined as shown below:

res.set('Content-Type', 'text/html');
res.send(new Buffer('<p>some html</p>'));

// When the parameter is a String, the method sets
// the Content-Type to “text/html”:
res.send('<p>some html</p>');

// When the parameter is an Array or Object,
// Express responds with the JSON representation:
res.send({ user: 'tobi' });
res.send([1,2,3]);
```

### res.redirect([status,] path)

`status` defaults to "302 "Found".

```js
res.redirect(301, 'http://example.com');
res.redirect('/login');

// Redirects can be relative to the current URL. For example,
// from http://example.com/blog/admin/
// (notice the trailing slash), the following would redirect
// to the URL http://example.com/blog/admin/post/new.
// Redirecting to post/new from http://example.com/blog/admin
// (no trailing slash), will redirect to
// http://example.com/blog/post/new.
// Note: This is because last trailing slash means directory
// and without trailing slash means file
// which is replaced by new path.
res.redirect('post/new');

// If you were on http://example.com/admin/post/new,
// the following would redirect
// to http//example.com/admin/post:
res.redirect('..');

// A back redirection redirects the request back to
// the referer, defaulting
// to / when the referer is missing.
res.redirect('back');
```

### res.render(view , locals, callback)

Renders a `view` and sends the rendered HTML string to the client.

```js
// send the rendered view to the client
res.render('index');

// if a callback is specified, the rendered HTML
// string has to be sent explicitly
res.render('index', function(err, html) {
  res.send(html);
});

// pass a local variable to the view
res.render('user', { name: 'Tobi' }, function(err, html) {
});
```
