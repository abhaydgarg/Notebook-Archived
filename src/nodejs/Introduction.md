# Node.js

> Asynchronous and non-blocking I/O model (architecture).

## What is an asynchronous?

Async is combination of following concepts:

* Single threaded and never block the main thread.
* Non blocking I/O using epoll/kqueue which uses [asynchronous I/O model](/computer-science/types-of-io/).
* Event loop.

## Why node.js

### Cost of I/O & non-blocking I/O

> Server - More I/O bound than CPU bound. Maximum job is related to I/O than CPU. So node.js primarily goal is to have non-blocking I/O and do not block/pause main thread.

What makes node.js so popular and set it apart from other web programming paradigms? In most software systems every system call, like reading or writing a file, querying a database, is blocking. i.e, the program execution will stop and wait for the call to finish and return its result. After that the program execution resumes. This is blocking I/O and synchronous programming. The more your program waits for the result, the more costly it is and this is cost of I/O… One of the core point of Node.js is to reduce this cost (the cost of waiting for I/O).

Current servers like Apache is multi threaded program, i.e.; it spawns a thread per request. When the program execution is stopped (by pending I/O) the thread / process will be put to sleep by the system but it will still consume resources. This is not a concern for single user (or low number of users) systems. But if you have a multi user system with a large number of users you are bound to hit the bottleneck! Each request will have a process or thread to handle them and these might be waiting for an I/O operation to be complete. So, till the task is complete the processes consume CPU and memory making it expensive. Node.js takes a different approach to solve this problem. It serves all the requests from a single thread.

The program code running in this thread is still executed synchronously but every time a system call takes place it will be delegated to the event loop along with a callback function. The main thread is not put to sleep and keeps serving other requests. As soon as the previous system call is completed, the event loop will execute the callback passed. This callback will usually deal with the result returned and continue the program flow. Thus, the main program is not blocked by the I/O operations i.e. Non-Blocking I/O and asynchronous programming!

### Event driven model vs. thread per request

The origin for this concern usually stems from the dedicated thread per request architecture found in traditional web servers. It steers you away from thinking how Node.js works by interleaving small events.

![Apache](assets/apache.jpg)

> Apache processing model uses dedicated workers.

The dedicated thread per request architecture is the way Apache, Tomcat, IIS and many other web servers or web application containers work. In these servers there is a predetermined amount of workers that handle all incoming requests. Each worker takes a single request and serves it completely from start to finish. It is responsible for reading the resources it needs, waiting for the reads to complete, doing any processing and returning a result. New requests go into waiting pattern until some worker is ready to take them.

Node.js works differently. All requests are taken in as they arrive. Instead of having multiple workers serving many requests there is only one thread. Each request processing gets the whole thread for itself for the time being. The catch is that as soon as some resource has to be waited on, you let go of the thread and allow others to use it. This is another way of expressing that Node.js performs I/O in an asynchronous and non-blocking way.

Processing each request until some resource has to be waited on splits the processing into small chunks. The code executes until an asynchronous operation needs to be performed. This could be for example a file read or a database call. The asynchronous operation is set up, provided with a means to continue and pass its results later on, and then the thread is let go for others to use.

During waiting time Node.js allows other requests to proceed. In a similar way another request may be processed until it needs to wait on something. This continues until the results for the original operation are in and the thread is free for us to continue processing the results.

The end result is that the chunks from different requests interleave. Processing of all requests advance a tiny bit at a time. While one request is waiting for something, others can be advanced, until they yet wait for something.

![Node.js processing model advances all requests concurrently.](assets/node.jpg)

> Node.js processing model advances all requests concurrently.

Compared to traditional web-serving techniques where each connection (request) spawns a new thread, taking up system RAM and eventually maxing-out at the amount of RAM available, Node.js operates on a single-thread, using non-blocking I/O calls, allowing it to support tens of thousands of concurrent connections held in Event Loop.

**A quick calculation:**

Assuming that each thread potentially has an accompanying 2 MB of memory with it, running on a system with 8 GB of RAM puts us at a theoretical maximum of 4000 concurrent connections, plus the cost of context-switching between threads. That's the scenario you typically deal with in traditional web-serving techniques. By avoiding all that, Node.js achieves scalability levels of over 1M concurrent connections.

_Performance is the main advantage, node.js allocates a small heap per each connection, while other server side solutions create a (2MB) thread for each incoming connection, and of course creating a thread is much slower than allocating heap memory._

## Why javascript for node?

1. JavaScript has closures and first-class functions (function as parameter registered as callback), which makes it a powerful match for event-driven/asynchronous programming.
2. Javascript automatically maintain the state of closures behind scene and programmer need not to worry about it. Closures are functions that inherit variables from their enclosing environment. When you pass a function callback as an argument to another function that will do I/O, the callback function will be invoked later, and this function will ' almost magically ' remember the context in which it was declared, along with all the variables available in that context and any parent contexts. This powerful feature is at the heart of Node's success. This shows that by using the closure pattern, you can have the best of both worlds: You can do event-driven programming without having to maintain the state by passing it around to functions. A JavaScript closure keeps the state for you.
