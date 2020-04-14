# Request Object

## Properties

### req.app

Holds a reference to the instance of the Express application.

### Paths

```js
// GET 'http://www.example.com/admin/new'

  //'/admin/new' - Note: req.originalUrl = req.baseUrl + req.path
  console.log(req.originalUrl);
  console.log(req.baseUrl); // '/admin'
  console.log(req.path); // '/new'
```

### req.query

```js
// url = /?q=js&src=typd
request.query = {q:'js', src:'typd'}

request.query.q // 'js'
```

### req.params

Fetch URL parameter.

```js
// url = /user/:name/:status = /user/abhay/active

request.params = {name:'abhay', status:'active'}

request.params.name // 'abhay'
```

### req.body

Contains key-value pairs of data submitted in the request body. By default, it is undefined, and is populated when you use body-parsing middleware such as `body-parser` and `multer`.

_Used for file upload and post form data._

```js
var app = require('express')();
var bodyParser = require('body-parser');
var multer = require('multer'); // v1.0.5
var upload = multer(); // for parsing multipart/form-data

app.use(bodyParser.json()); // for parsing application/json
// for parsing application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: true }));

app.post('/profile', upload.array(), function (req, res, next) {
  console.log(req.body);
  res.json(req.body);
});
```

> `request.param()` method is deprecated use `request.param`, `req.body` and `req.query`
