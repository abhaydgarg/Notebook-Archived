# HTTP Polling, Long Polling, Server-sent Events (SSE), and WebSockets

> Polling, Long Polling, Server-sent Events and WebSockets exist to solve the problem of real time updates in web applications.

## Polling

> One way - Browser initate.

Every few seconds your web application makes a HTTP request, usually GET or POST, to the server checking for updates.

## Long Polling

> One way - Browser initate.

The browser still makes requests to the server for updates, but the nature of when and why is very different.

The idea is that each request, by the web app, to the server will have something like a timestamp with it. The timestamp value is then used to compare the data's current timestamp on the server. If the server sees that the request's timestamp is older than the current timestamp for the data, then the server responds with the updated data and new timestamp immediately and ends the response. However, if the server sees that the timestamp is the same as the data's current time stamp, then it simply holds off on responding to it until the data's timestamp changes. When the web app gets the response, it will immediately make another request with the new timestamp.

Its completely HTTP, without anything new. And because, a very long time ago, HTTP/1.1 introduced a field ( referred to as the "Keep-Alive" mechanism ) as part of the request headers asking the server to keep the TCP connection alive, each of those requests will usually be able to recycle the same TCP connection. Which means, in the most common case, no 3-way handshake overhead.

If well implemented, it can be very effective at providing genuinely real time updates to users.

## WebSockets

> Two way - Browser and server both initiate.

The WebSocket API is an advanced technology that makes it possible to open a two-way interactive communication session between the user's browser and a server. With this API, you can send messages to a server and receive event-driven responses without having to poll the server for a reply.

## Server-sent Events (SSE)

> One way - Server initate.

A server-sent event is when a web page automatically gets updates from a server.

This was also possible before, but the web page would have to ask if any updates were available. With server-sent events, the updates come automatically without asking by browser.
