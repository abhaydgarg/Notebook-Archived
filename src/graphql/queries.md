# Queries

## Different parts of query

```gql
query getUsers {
  users {
    id
    firstName
    lastName
    phone
    username
  }
}
```

* **Operation type** (query): the type of action we want to do with the query. We have three different operation types - **query**, **mutation** and **subscription**.

* **Operation name** (getUsers): name is important for debugging purposes.

* **Selection set** (id, firstName, lastName, phone, username): this defines what data we would like to retrieve from the server.

## Comments

```gql
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```

## Arguments

> Every field and nested object can get its own set of arguments.

```gql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

## Aliases

```gql
query getUsers {
  users(role: admin) {
    id
    firstName
    lastName
    phone
    username
  }
  users(role: accountant) {
    id
    firstName
    lastName
      phone
    username
  }
}
```

Let’s say that we have a schema for users, where the role argument is implemented and we want to query the users based on that role argument. However, the above example will return an error, because you are using `users(role: admin)` and `users(role: accountant)` which have same name `users`. You have same selection set which is going to fetch from Graphql server having same name.

Using aliases can solve the above problem.

```gql
query getUsers {
  admins: users(role: admin) {
    id
    firstName
    lastName
    phone
    username
  }
  accountants: users(role: accountant) {
    id
    firstName
    lastName
    phone
    username
  }
}
```

## Fragments

> A fragment is basically a reusable piece of query.

In GraphQL, you often need to query for the same data fields in different queries. The code to define these fields has to be written multiple times, leading to more errors. By reusing this code, we can be more efficient with our time and reuse these pieces of query logic on different queries.

```gql
query getUsers {
  admins: users(role: admin) {
    id
    firstName
    lastName
    phone
    username
  }
  accountants: users(role: accountant) {
    id
    firstName
    lastName
    phone
    username
  }
}
```

We can see, there’s still room for improvement, as the fields of each query are repeated multiple times. Since they are the same selection set, we can define the fields just once then reference them as needed.

```gql
fragment userFields on User {
   id
   firstName
   lastName
   phone
   username
}
```

Each fragment contains the name of the fragment `(userFields)`, to what type we are applying this fragment `(User)` and the selection set `(id, firstName, lastName, phone and username)`.

Now we can rewrite the getUsers query with the fragment and spread operator.

```gql
query getUsers {
  admins: users(role: admin) {
    ...userFields
  }
  accountants: users(role: accountant) {
    ...userFields
  }
}

fragment userFields on User {
   id
   firstName
   lastName
   phone
   username
}
```

## Variables

```gql
query ($username: String!) {
  reddit {
    user(username: $username) {
      username
      commentKarma
      createdISO
    }
  }
}
```

In plain-English, this query says it accepts one variable (username), and it is a required string.

* All of the variables needed for this query are declared up-front, so your tooling can quickly throw an error if you forget to pass a value.

* All variables must be used in the query — if a variable is declared but not used, GraphQL regards the query as invalid.

### How do we set the values for variables

The GraphQL-JS ecosystem generally uses a separate JSON blob called `variables`:

```gql
POST https://www.graphqlhub.com/graphql
{
  "query":"query ($username: String!) {
    reddit {
      user(username: $username) {
        username
        commentKarma
        createdISO
      }
    }
  }",
  "variables":"{
    "username": "kn0thing"
  }"
}
```

### Default value

```gql
query ($username: String = "GovSchwarzenegger"){
  reddit {
    user(username: $username) {
      username
      commentKarma
      createdISO
    }
  }
}
```

## Directives

### @include(if: Boolean)

> Only include this field in the result if the argument is `true`.

```gql
# return awesomeField only if $isAwesome is true
query myAwesomeQuery($isAwesome: Boolean) {
  awesomeField @include(if: $isAwesome)
}
```

### @skip(if: Boolean)

```gql
# return awesomeField only if $isAwesome is false
query myAwesomeQuery($isAwesome: Boolean) {
  awesomeField @skip(if: $isAwesome)
}
```

## Mutations

Technically we can modify data on the server using a query. However, the common accepted convention is that every operation that includes writes should be sent using a mutation, hence the name of the keyword.

> Use `mutation` instead of `query` - Operation type

```gql
# Note how createReview field returns the stars and commentary
# fields of the newly created review.
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

# variables
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```
