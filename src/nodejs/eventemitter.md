# EventEmitter

Most of Node’s objects — like HTTP requests, responses, and streams — implement the `EventEmitter` module so they can provide a way to emit and listen to events.

> Using EventEmitter, you can establish the communication between objects. And it is a great way to run multiple functions (listeners) on single event.

The concept is simple: emitter objects emit named events that cause previously registered listeners to be called. So, an emitter object basically has two main features:

1. Emitting name events.
2. Registering and unregistering listener functions.

```js
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter()
// Emit action.
myEmitter.emit('something-happened')
// Attach listener using on method.
myEmitter.on('something-happened', () => console.log('Hi!'))
```

## Events !== Asynchrony

```js
const EventEmitter = require('events');

class WithLog extends EventEmitter {
  execute(taskFunc) {
    console.log('Before executing');
    this.emit('begin');
    taskFunc();
    this.emit('end');
    console.log('After executing');
  }
}

const withLog = new WithLog();

withLog.on('begin', () => console.log('About to execute'));
withLog.on('end', () => console.log('Done with execute'));

withLog.execute(() => console.log('*** Executing task ***'));

// There is nothing asynchronous about this code.
// Do not assume that events mean synchronous
// or asynchronous code.

/*
Before executing
About to execute
*** Executing task ***
Done with execute
After executing
*/
```

This is important, because **if we pass an asynchronous `taskFunc` to execute, the events emitted will no longer be accurate.**

```js
// ...

withLog.execute(() => {
  setImmediate(() => {
    console.log('*** Executing task ***')
  });
});

/*
Before executing
About to execute
Done with execute
After executing
*** Executing task ***
*/
```

## Asynchronous event

```js
const fs = require('fs');
const EventEmitter = require('events');

class WithTime extends EventEmitter {
  execute(asyncFunc, ...args) {
    this.emit('begin');
    console.time('execute');
    asyncFunc(...args, (err, data) => {
      if (err) {
        return this.emit('error', err);
      }

      this.emit('data', data);
      console.timeEnd('execute');
      this.emit('end');
    });
  }
}

const withTime = new WithTime();

withTime.on('begin', () => console.log('About to execute'));
withTime.on('end', () => console.log('Done with execute'));

withTime.execute(fs.readFile, __filename);

/*
About to execute
execute: 4.507ms
Done with execute
*/
```

```js
// Using async/await.
class WithTime extends EventEmitter {
  async execute(asyncFunc, ...args) {
    this.emit('begin');
    try {
      console.time('execute');
      const data = await asyncFunc(...args);
      this.emit('data', data);
      console.timeEnd('execute');
      this.emit('end');
    } catch(err) {
      this.emit('error', err);
    }
  }
}
```

## `error` and `data` events

These two are special events which is by convention should be used to represent actions error and data. For example,

```js
// To handle data.
eventObj.on('data', (data) => {
  // do something with data
});

// To handle error.
eventObj.on('error', (err) => {
  console.log(err)
});
```

## Order of listeners

If we register multiple listeners for the same event, the invocation of those listeners will be in order. The first listener that we register is the first listener that gets invoked.

> Note: If you need to define a new listener, but have that listener must be invoked first, you can use the `prependListener` method.

```js
// प्रथम
withTime.on('data', (data) => {
  console.log(`Length: ${data.length}`);
});

// दूसरा
withTime.on('data', (data) => {
  console.log(`Characters: ${data.toString().length}`);
});

withTime.execute(fs.readFile, __filename);

/*
Length:
Characters:
*/
```
