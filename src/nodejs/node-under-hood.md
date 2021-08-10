# Node under hood

## What run on main thread and what does not

### [1]

All Javascript, V8 and Event loop run in main thread.

### [2]

C++ has access to main thread and some javascript methods are backed by C++ that means if you call those javascript methods in node.js then their actual execution is done in C++ realm such as `fs` methods, so when C++ done execution then it send result back to javascript method. It is like going back and forth from javascript to C++ to handle certain tasks. There are two cases here when C++ backed javascript methods are called in node.js:

#### [2.1] If a synchronous javascript method is called then C++ will always run on the main thread.

```js
  var fs = require('fs');
  var contents = fs.readFileSync('file.txt', 'utf8');
  // this line is not reached until the read results are in
  console.log(contents);
```

Above `fs` method is synchronous and backed by C++. So it runs in main thread which block the main thread until the I/O is completed.

#### [2.2] If an asynchronous javascript method is called then again there are two cases:

##### [2.2.1]

Uses main thread using asynchronous non-blocking system calls: C++ uses **kernel async** (epoll/kqueue) form `libuv` lib using main thread but do not block the main thread because epoll/kqueue system call do not pause the main thread by putting it in sleep while waiting for I/O. epoll/kqueue uses [Asynchronous I/O](/computer-science/types-of-io/) model which does not block main thread.

###### What handles by kernel async using asynchronous non-blocking system calls

- Pipe
- TCP
- TTY (console/bash/terminal)
- UDP

##### [2.2.2] Uses worker thread: **Threads** from thread pool.

> In thread pool there are only 4 threads. If in case all worker threads are busy then node waits until any out of 4 threads get free.

```js
var fs = require('fs');
fs.readFile('file.txt', 'utf8', function(err, data) {
  // called at a later time when the results are in
  console.log(data)
});
// readFile returns immediately and this line is reached right away
```

Above `fs` method is an asynchronous and backed by C++. So it does not run in main thread and the node executes it using one of the worker threads from thread pool. So it does not block the main thread.

###### What handles by worker threads

- DNS
- Filesystem

## Non-blocking system calls

![Non blocking system calls](assets/diagram.png)

One way of making a blocking system call non-blocking is to perform it in a separate thread. This way the main thread is free to continue working on other things as the blocking operation is blocking a completely different thread. This is the solution Node.js uses for blocking system calls.

Not every system call is blocking. Asynchronous processing model is known to be performant. And this fact is know at the operating system level, too.

There are non-blocking system call implementations available for some operations. They aren't widespread and available for every possible operation, but there are some. And Node.js can make use of these.

Similar to JavaScript, a non-blocking call is separates its initiation and passing of results. Whereas a callback is used in JavaScript, a different more pro-active approach called polling is used at the operating system level.
