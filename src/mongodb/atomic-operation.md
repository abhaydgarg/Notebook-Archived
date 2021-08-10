# Atomic operation

## findAndModify()

Multiple operation in single query that is called atomic operation.

To achieve atomicity it is recommended to keep all the related information which is frequently updated together in a single document using embedded documents. In other words, atomic operation work more efficiently and fast if the multiple operations are executed on single document. Lets see the example below:

```js
{
   "_id":1,
   "product_name": "Samsung S3",
   "category": "mobiles",
   "product_total": 5,
   "product_available": 3,
   "product_bought_by": [
    {
      "customer": "john",
      "date": "7-Jan-2014"
    },
    {
      "customer": "mark",
      "date": "8-Jan-2014"
    }
   ]
}
```

In this document, we have embedded the information of customer who buys the product in the product\_bought\_by field. Now, whenever we a new customer buys the product, we will first check if the product is still available using product\_available field. If available, we will reduce the value of product\_available field as well as insert the new customer's embedded document in the product\_bought\_by field. We will use findAndModify command for this functionality because it searches and updates the document in the same go.

```js
db.products.findAndModify({
   query:{_id:2,product_available:{$gt:0}},
   update:{
      $inc:{product_available:-1},
      $push:{product_bought_by:{customer:"rob",date:"9-Jan-2014"}}
   }
})
```

Our approach of embedded document and using findAndModify query makes sure that the product purchase information is updated only if it the product is available. And the whole of this transaction being in the same query, is atomic.
