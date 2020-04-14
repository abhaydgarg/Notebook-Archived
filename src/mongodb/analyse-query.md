# Analyzing queries

The `explain()` method provides information on the query, indexes used in a query and other statistics. It is very useful when analyzing how well your indexes are optimized.

```js
db.users.find({gender:"M"},{user_name:1,_id:0}).explain()
```

The `hint()`operator forces the query optimizer to use the specified index to run a query. This is particularly useful when you want to test performance of a query with different indexes.

```js
db.users.find({gender:"M"},{user_name:1,_id:0}).hint({gender:1,user_name:1});

/** To analyze the above query using $explain: **/

db.users.find({gender:"M"},{user_name:1,_id:0}).hint({gender:1,user_name:1}).explain()
```
