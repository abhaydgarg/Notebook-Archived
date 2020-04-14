# MongoDB

## Collection and document

MongoDB uses the `Collection` and `Document` concept for data modeling.

| RDBMS | MongoDB |
| --- | --- |
| Database | Database |
| Table | Collection |
| Row | Document (Object) |
| Column | Field (Object Key) |
| Table Join | Embedded Documents, Reference |
| Primary Key | key "\_id" |

## Advantages of MongoDB over RDBMS

* Schema less (Flexible, Dynamic schema) It is not mandatory to have pre-defined document structure where we have specific keys already defined but instead in a collection we have documents (objects) having any number of keys and can have different types of keys. In RDBMS, we must have same number of columns and their types shared by each row. They cannot be different for each row.

* Document Oriented Storage Data is stored in the form of JSON style documents.

* Auto-Sharding Database partitioning that separates very large databases the into smaller, faster, more easily managed parts called data shards. The word shard means a small part of a whole.

* Advantage of Map and Reduce. MapReduce can be used for batch processing of data and aggregation operations.

## Why MongoDB was built

It is built for three main reasons high performance, high availability, and automatic scaling.

**High Performance and Easy to Use** Support for embedded data models reduces I/O activity on database system. Less joins mean by single query data can be fetched because join query as in case of RDBMS require to operate on multiple tables to fetch data which has impact on performance.

**High Availability** MongoDB servers that maintain the same data set (Replication), providing redundancy but increasing data availability during failover.

**Horizontal Scalability** Sharding distributes data across a cluster of machines make DB to maintain easily when it scale that means when DB increases in its size and lot of operations are running on DB. `Scalability is the capability of a system to handle a growing amount of work, or its potential to be enlarged in order to accommodate that growth. Scalability means the ability to handle growth.`

## When to use and when not

1. Do not have too many relations between documents. MongoDB does not support joins. In MongoDB some data is denormalized, or stored with related data in documents called embedded document to remove the need for joins. You can have a Database Reference, but they're not enough. **Use in the cases (the business model) where a no or very less need of join or linking or relation is required between documents.**

2. For apps which has almost no relational data and needs scalability are the perfect fit of MongoDB. Most developers also use MongoDB to store related data and do a join manually in their code, which might work in certain scenarios which has a one level join / less relations between data but not for all.

3. Not suitable for applications that require complex, multi-row transactions. That means the situation where DB has to interact or communicate between number of tables to get result. A concrete example would be the booking engine behind a travel reservation system, which also typically involves complex transactions. To make it blindingly fast, massively scalable, and easy to use , the team had to leave some heavy features behind, which means that MongoDB is not an ideal candidate for certain situations. For example, its lack of transaction or relational support means that you wouldn't want to use MongoDB to write an accounting application.

4. Use it when hierarchical deepness is at one level. That means if there is a need of relation then it should be at level one.

## What's MongoDB good for

* **Account and user profiles:** can store arrays of addresses with ease
* **CMS:** the flexible schema of MongoDB is great for heterogeneous collections of content types
* **Form data:** MongoDB makes it easy to evolve the structure of form data over time
* **Blogs / user-generated content:** can keep data with complex relationships together in one object
* **Log data of any kind:** structured log data is the future
* **Location based data:** MongoDB understands geo-spatial coordinates and natively supports geo-spatial indexing
