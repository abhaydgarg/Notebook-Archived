# EJS

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
