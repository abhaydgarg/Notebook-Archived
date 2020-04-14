# CRLF

The term CRLF refers to Carriage Return (ASCII 13, \r) Line Feed (ASCII 10, \n). They're used to note the termination of a line, however, dealt with differently in today’s popular Operating Systems. For example: in Windows both a CR and LF are required to note the end of a line, whereas in Linux/UNIX a LF is only required. In the HTTP protocol, the CR-LF sequence is always used to terminate a line.

When a browser sends a request to a web server, the web server answers back with a response containing both the HTTP headers and the actual website content. The HTTP headers and the HTML response (the website content) are separated by a specific combination of special characters, namely a carriage return and a line feed. For short they are also known as CRLF.

The server knows when a new header begins and another one ends with CRLF, which can also tell a web application or user that a new line begins in a file or in a text block.

## What is the CRLF injection vulnerability?

In a CRLF injection vulnerability attack the attacker inserts carriage return, linefeed both of the characters into user input to trick the server, web application or the user into thinking that an object is terminated and another one has started.

In web applications a CRLF injection can have severe impacts, depending on what the application does with single items. Impacts can range from information disclosure to code execution. For example it is also possible to manipulate log files in an admin panel as explained in the below example.

## Response splitting example

Since the header of a HTTP response and its body are separated by CRLF characters an attacker can try to inject those. A combination of CRLFCRLF will tell the browser that the header ends and the body begins. That means that he is now able to write data inside the response body where the html code is stored. This can lead to a Cross-site Scripting vulnerability.

Imagine an application that sets a custom header, for example:

`X-Your-Name: Bob`

The value of the header is set via a get parameter called "name". If no URL encoding is in place and the value is directly reflected inside the header it might be possible for an attacker to insert the above mentioned combination of CRLFCRLF to tell the browser that the request body begins. That way he is able to insert data such as XSS payload, for example:

`?name=Bob%0d%0a%0d%0a<script>alert(document.domain)</script>`

The above will display an alert window in the context of the attacked domain.

## HTTP header injection

By exploiting a CRLF injection an attacker can also insert HTTP headers which could be used to defeat security mechanisms such as a browser's XSS filter or the same-origin-policy. This allows the attacker to gain sensitive information like CSRF tokens. He can also set cookies which could be exploited by logging the victim in the attacker's account or by exploiting otherwise unexploitable.

If an attacker inserts the headers that would activate CORS (Cross Origin Resource Sharing), he can use javascript to access resources that are otherwise protected by SOP (Same Origin Policy) which prevents sites from different origins to access each other.

## Prevention

The best prevention technique is to not let users supply input directly inside response headers. If that is not possible, you should always use a function to encode the CR and LF special characters. It is also advised to update your programming language to a version that does not allow CR and LF to be injected inside functions that set headers.
