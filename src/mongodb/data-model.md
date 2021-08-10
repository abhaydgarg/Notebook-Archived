# Data model

## Normalized Data Model - References

Database normalization (or normalisation) is the process of organizing the data in such a way that minimize data redundancy. Mostly used in RDBMS.

In normalized data model, references store the relationships between data by including links or references from one document to another. Applications can resolve these references to access the related data.

1. Reduce data redudancy.
2. Higly and effiecient use of join is required.
3. Increase number of DB query to read/write data because data is requested from multiple sources.

![RDBMS Schema for Blog](assets/data-model-normalized.png)

### Use reference data models when

1. when embedding would result in duplication of data but would not provide sufficient read performance advantages to outweigh the implications of the duplication.
2. to represent more complex many-to-many relationships.
3. to model large hierarchical data sets.
4. One-to-Many Relationships with Document References.

Consider the following example that maps publisher and book relationships. The example illustrates the advantage of referencing over embedding to avoid repetition of the publisher information.

Embedding the publisher document inside the book document would lead to repetition of the publisher data, as the following documents show:

```js
{
   title: "MongoDB: The Definitive Guide",
   author: [ "Kristina Chodorow", "Mike Dirolf" ],
   published_date: ISODate("2010-09-24"),
   pages: 216,
   language: "English",
   publisher: {
     name: "O'Reilly Media",
     founded: 1980,
     location: "CA"
   }
}

{
   title: "50 Tips and Tricks for MongoDB Developer",
   author: "Kristina Chodorow",
   published_date: ISODate("2011-05-06"),
   pages: 68,
   language: "English",
   publisher: {
     name: "O'Reilly Media",
     founded: 1980,
     location: "CA"
   }
}
```

To avoid repetition of the publisher data, use references and keep the publisher information in a separate collection from the book collection.

```js
{
   _id: "oreilly",
   name: "O'Reilly Media",
   founded: 1980,
   location: "CA"
}

{
   _id: 123456789,
   title: "MongoDB: The Definitive Guide",
   author: [ "Kristina Chodorow", "Mike Dirolf" ],
   published_date: ISODate("2010-09-24"),
   pages: 216,
   language: "English",
   publisher_id: "oreilly"
}

{
   _id: 234567890,
   title: "50 Tips and Tricks for MongoDB Developer",
   author: "Kristina Chodorow",
   published_date: ISODate("2011-05-06"),
   pages: 68,
   language: "English",
   publisher_id: "oreilly"
}
```

## Denormalized Data Model - Embedded Data

Embedded documents capture relationships between data by storing related data in a single document structure. MongoDB documents make it possible to embed document structures in a field or array within a document. These denormalized data models allow applications to retrieve and manipulate related data in a single database operation.

1. Increase data redundancy.
2. No need of join is required.
3. Single DB query to read/write data because data is requested from single source.
4. Atomicity of query. **That means in one query you can perform multiple operations like insert and update together.** A denormalized data model with embedded data combines all related data for a represented entity in a single document. This facilitates atomic write operations since a single write operation can insert or update the data for an entity. Normalizing the data would split the data across multiple collections and would require multiple write operations that are not atomic collectively.

![RDBMS Schema for Blog](assets/data-model-denormalized.png)

### Use embedded data models when, you have

#### One-to-One relationships with embedded documents

Consider the following example that maps patron and address relationships. The example illustrates the advantage of embedding over referencing if you need to view one data entity in context of the other. In this one-to-one relationship between patron and address data, the address belongs to the patron.

In the normalized data model, the address document contains a reference to the patron document.

```js
{
   _id: "joe",
   name: "Joe Bookreader"
}

{
   patron_id: "joe",
   street: "123 Fake Street",
   city: "Faketon",
   state: "MA",
   zip: "12345"
}
```

If the address data is frequently retrieved with the name information, then with referencing, your application needs to issue multiple queries to resolve the reference. The better data model would be to embed the address data in the patron data, as in the following document:

```js
{
   _id: "joe",
   name: "Joe Bookreader",
   address: {
     street: "123 Fake Street",
     city: "Faketon",
     state: "MA",
     zip: "12345"
   }
}
```

With the embedded data model, your application can retrieve the complete patron information with one query.

#### One-to-Many relationships with embedded documents

In these relationships the "many" or child documents always appear with or are viewed in the context of the "one" or parent documents.

```js
/** NORMALIZED **/

{
   _id: "joe",
   name: "Joe Bookreader"
}

{
   patron_id: "joe",
   street: "123 Fake Street",
   city: "Faketon",
   state: "MA",
   zip: "12345"
}

{
   patron_id: "joe",
   street: "1 Some Other Street",
   city: "Boston",
   state: "MA",
   zip: "12345"
}


/** DENORMALIZED **/

{
   _id: "joe",
   name: "Joe Bookreader",
   addresses: [
    {
      street: "123 Fake Street",
      city: "Faketon",
      state: "MA",
      zip: "12345"
    },
    {
      street: "1 Some Other Street",
      city: "Boston",
      state: "MA",
      zip: "12345"
    }
  ]
 }
```
