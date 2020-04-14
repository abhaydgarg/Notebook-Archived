# Streams

## Buffering vs streaming

Buffering - For an I/O operations, the buffer mode causes all the data coming from a resource to be collected into a buffer; it is then passed to a callback as soon as the entire resource is read.

Streaming - On the other side, streams allow you to process the data as soon as it arrives from the resource.

### Benefits

1. **Space efficiency** - Lets say you need to read a very big file, so if you use buffer, you need to read the file completely before processing which is not a good idea because you will soon run out of memory. Thus for that you need to use streams.

2. **Time efficiency** - Lets say you need to compress a file and upload it to the server which will decompress the file. So if you use buffer it will read the entire data and then compress it similarly decompression on the server will start only when the data has been received. So to minimize the time you can use streams which will compress small data chunks and send to the server. Server will also decompress small chunks of data rather than full buffer.

## When to use Streams

When reading a file synchronously with `fs.readFileSync` , the program will block, and all of the data will be read to memory. Using `fs.readFile` will prevent the program from blocking because it's an asynchronous method, but it'll still read the entire file into memory. What if there were a way to tell `fs.readFile` to read a chunk of data into memory, process it, and then ask for more data? That's where streams come in. Memory becomes an issue when working with large files--compressed backup archives, media files, large log files, and so on. Instead of reading the entire file into memory, you could use `fs.read` with a suitable buffer, reading in a specific length at a time. Or, preferably, you could use the streams API provided by `fs.createReadStream` .

## Types of Streams

1. A **readable** stream is one that you can read data from but not write to. A good example of this is `process. stdin`, which can be used to stream data from the standard input.
2. A **writable** stream is one that you can write to but not read from. A good example is `process.stdout`, which can be used to stream data to the standard output.
3. A **duplex** stream is one that you can both read from and write to. A good example of this is the network socket. You can write data to the network socket as well as read data from it.
4. A **transform** stream is a special case of a duplex stream where the output of the stream is in some way computed from the input. These are also called through streams. A good example of these is encryption and compression streams.

Each type of Stream is an **EventEmitter** instance and throws several events at different instance of times. For example, some of the commonly used events are:

- **data** - This event is fired when there is data is available to read.
- **end** - This event is fired when there is no more data to read.
- **error** - This event is fired when there is any error receiving or writing data.
- **finish** - This event is fired when all data has been flushed to underlying system

## Reading

```js
var fs = require("fs");
var data = '';

// Create a readable stream
var readerStream = fs.createReadStream('input.txt');

// Set the encoding to be utf8\.
readerStream.setEncoding('UTF8');

// Handle stream events --> data, end, and error
readerStream.on('data', function(chunk) {
  data += chunk;
});

readerStream.on('end', function() {
  console.log(data);
});

readerStream.on('error', function(err) {
  console.log(err.stack);
});
```

## Writing

```js
var fs = require("fs");
var data = 'Simply Easy Learning';

// Create a writable stream
var writerStream = fs.createWriteStream('output.txt');

// Write the data to stream with encoding to be utf8
writerStream.write(data, 'UTF8');

// Mark the end of file
writerStream.end();

// Handle stream events --> finish, and error
writerStream.on('finish', function() {
  console.log("Write completed.");
});

writerStream.on('error', function(err) {
  console.log(err.stack);
});
```

## Piping

Piping is a mechanism where we provide output of one stream as the input to another stream. It is normally used to get data from one stream and to pass output of that stream to another stream. There is no limit on piping operations. Now we'll show a piping example for reading from one file and writing it to another file.

```js
var fs = require("fs");

// Create a readable stream
var readerStream = fs.createReadStream('input.txt');

// Create a writable stream
var writerStream = fs.createWriteStream('output.txt');

// Pipe the read and write operations
// read input.txt and write data to output.txt
readerStream.pipe(writerStream);
```

### Chaining Stream - commonly used in piping

```js
var fs = require("fs");
var zlib = require('zlib');

// Compress the file input.txt to input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));

console.log("File Compressed.");
```
