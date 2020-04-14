# Data manipulation

## How sorting work when no sort order is given in query

The **natural order** is the database's native ordering method for objects within a (normal) collection. When you query for items in a collection without specifying an explicit sort order, the items are returned by default in forward natural order. This may initially appear identical to the order in which items were inserted; however, the natural order for a normal collection is not defined and may vary depending on document growth patterns, indexes used for a query, and the storage engine used.

## Capped Collections

They are a fixed size. Once a capped collection is full, the oldest data will be purged and newer data will be added at the end, ensuring that the natural order follows the order in which the records were inserted. This type of collection can be used for logging and auto archiving data. The natural order is guaranteed to be the order in which the documents were inserted.

What if you want the items in reverse order of in which the documents were inserted?

Use `$natural` with value -1.

```js
db.audit.find().sort({ $natural: -1 });
```

## AND vs OR

MongoDB during find query by default uses `AND`. If want to use `OR` then you have to mention it in query by `$or` operator.

## Multiple ways to write find() query

```js
db.media.find ( { "Released" : { $lt : 1995 } } );
db.media.find ( { $where: "this.Released < 1995" } );
db.media.find ("this.Released < 1995");

f = function() { return this.Released < 1995 };
db.media.find(f);
```

### Projection

In mongodb projection meaning is selecting only necessary data rather than selecting whole of the data of a document. If a document has 5 fields and you need to show only 3, then select only 3 fields from them.

The second parameter of find() method where you tell mongoDB the fields you want and do not want in a result.

```js
/** 1 is used to show the field while 0 is used to hide the field. **/

db.media.find({},{"title":1,_id:0});

//result
{"title":"MongoDB Overview"}
{"title":"NoSQL Overview"}
{"title":"Tutorials Point Overview"}
```

## Bulk Operation

Bulk write operations can be either ordered or unordered.

With an **ordered list** of operations, MongoDB executes the operations serially. If an error occurs during the processing of one of the write operations, MongoDB will return without processing any remaining write operations in the list.

With an **unordered list** of operations, MongoDB can execute the operations in parallel, but this behavior is not guaranteed. If an error occurs during the processing of one of the write operations, MongoDB will continue to process remaining write operations in the list.

## Using Reference (Relation)

### Manually Reference

Where you save the `_id` field of one document in another document as a reference. Then your application can run a second query to return the related data. These references are simple and sufficient for most use cases.

The only limitation of manual linking is that these references do not convey the database and collection names. This is where you can use another approach which is called:

### DBRefs

```js
{ $ref : <collectionname>, $id : <id value>[, $db : <database name>] }
```

So, by viewing the object data we can tell the relationship of object with which collection and DB. In manual we do not know this by just looking at the object data. It should be obvious through the field name in the case of manual.
