# GridFS

GridFS is a feature for storing and retrieving files that exceed the BSON-document size limit of 16 MB.

Instead of storing a file in a single document, GridFS divides the file into parts, or chunks, and stores each chunk as a separate document. By default, GridFS uses a chunk size of 255 kB; that is, GridFS divides a file into chunks of 255 kB with the exception of the last chunk. The last chunk is only as large as necessary. Similarly, files that are no larger than the chunk size only have a final chunk, using only as much space as needed plus some additional metadata.

## When to Use GridFS

* Instead using system level filesystem to store the images, audio, videos etc. You want to store file in different chunks in BLOB format. The same way MYSQL store the file in table.
* When you want to access information from portions of large files without having to load whole files into memory, you can use GridFS to recall sections of files without reading the entire file into memory.
* When you want to keep your files and metadata automatically synced and deployed across a number of systems and facilities.

**GridFS stores files in two collections:**

* chunks stores the binary chunks. **(fs.chunks)**
* files stores the file's metadata. **(fs.files)**
