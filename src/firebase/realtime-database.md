# Realtime database

!!! info "Motto"
    Write should be hard and read should be easy.

## Understand DB

### Every piece of data should be object

**Question:** Why?

**[1]** The `key` in DB is reccomnded to be a `primary key`. In a example below, `john` and `jane` keys in `users` object are primary keys. If we compare it with relational DB then `users` is a table and it has two records and these records have a primary key `john` and `jane`.

```json
"users": {
  "john": {
    "name": "John Doe",
  },
  "jane": {
    "name": "Jane Doe",
  }
}
```

**[2]** The data or record is accessed using **object path**. For example, `/users/john`. This returns the whole record of user `john`. The reason using object path that the data is accessed extremly fast as compare to looking up record in an array.

**[3]** Array are not supported. We can create an array and push items in it but behind the scene firebase convert array into object. For example:

```js
// we send this
['hello', 'world']
// Firebase stores this
{0: 'hello', 1: 'world'}
```

**[4]** Firebase provides a very limited operations for read and write. It does not support joins and a very limited operation to query DB. Therefore, we have to structure the DB in key-value in way that we can access record efficiently.

## Best practices for data structure

### When to use nested (embedded) object

> Note: Firebase recommends to avoid using nested data but in a few cases it can be used.

#### Cardinality 1:1 + Static or fixed size

Both conditons must be true.

**[1] Cardinality 1:1** - When relation is one-directional. The data is not going to be redundant. For example, husband and wife is 1:1 relation so we can choose to embed wife inside husband record.

```json
"users": {
  "john": {
    "name": "John Doe",
    "age": 30,
    "wife": {
      "name": "Jane Doe",
      "age": 28,
    }
  }
}
```

**[2] Static or fixed size** - This means that the size of nested or embedded objects should be fixed or not in a large numbers. So what should be the size of nested objects?

This depends upon the amount of data which is going to be returned. For example, lets say we have `posts` object which has `comments` object embedded into it. The relation is 1:1 but what if the size of `comments` object increased to 1k, 5k or 10k over time. When you access `posts/one` then the comments will also be returned with it. This can be problematic when the size of `comments` object is large. Lets say we have constraint that user cannot submit more than 10 comments. In this case we can consider embedding `comments` object into `post` because size of `comments` object is small and fixed.

```json
"posts": {
  "one": {
    "title": "abc",
    "commnets": {
      "c1": "first comment",
      "c2": "second comment",
      "c3": "third comment",
    }
  }
}
```

### When to flatten the data - Denormalization

> Denormalization - It is just a fancy word of duplication.

#### Cardinality 1:M or M:M

When relationship is more dynamic and the nested object can grow and mutate heavily.

> IDEA: Split data into multiple object paths and use primary key of one object in another object as a foreign key.

Lets take an above example of `posts` object and separate the structure into multiple paths. Lets check the example below, the primary keys of `posts` such as `one`, `two` become a first level key in a `comments` object which you can think as foreign keys. So you can access single post using path `posts/one` and its comments using `comments/one`. Simple isn't.

**Benefit:** You can now get post's data of single post without having a long list of comments. Because earlier, in a old structure when you get a single post by path `posts/one` then you will also recieve all comments with it. So now if you just need metadata of single post then use `posts/one` and when want post's comment just use `comments/one`.

```json
"posts": {
  "one": {
    "title": "abc",
  },
  "two": {
    "title": "xyz",
  }
}

"commnets": {
  "one": {
    "c1": "first comment",
    "c2": "second comment",
    "c3": "third comment",
  },
  "two": {
    "c1": "first comment",
    "c2": "second comment",
    "c3": "third comment",
  }
}
```

##### Other ways to denormalize

###### Store relation in a separate object

To access a list of users who are going to attend event `e1`. You use a path `attendees/e1`.

```json
"users": {
  "john": {
    "name": "John Doe",
  },
  "jane": {
    "name": "Jane Doe",
  }
}

"events": {
  "e1": {
    "title": "Event One",
    "location": "some address",
  },
  "e2": {
    "title": "Event Two",
    "location": "some address",
  }
}

// users and events relation
// is stored here.
"attendees": {
  "e1": {
    "john": true,
    "jane": true,
  },
  "e2": {
    "jane": true,
  }
}
```

You can also embed the user object instead of `true` value.

**Drawback - Maintain consistency:** Embedding a object has drawback and which is, if you update the name of `jane` in `user` object then you have to update the same in all events in which `jane` is registred. Therfore, in `attendees` object you have to go through all events and update the `jane` object to match the changes.

> This is uselful when embedded object is not going to mutate. Therefore, you do not need to execute another query to fetch the content. unlike in example above, after you get the users of event `e1` whose value is `true`. You have to run two read statements using `users/$uid` to get the name of attendees.

```json
"attendees": {
  "e1": {
    "john": {
      "name": "John Doe",
    },
    "jane": {
      "name": "Jane Doe",
    },
  },
  "e2": {
    "jane": {
      "name": "Jane Doe",
    },
  }
}
```
