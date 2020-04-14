# Execution

GraphQL cannot execute a query without a type system, let's use an example type system to illustrate executing a query.

```gql
# Schema/type
type Query {
  human(id: ID!): Human
}

type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}

enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}

type Starship {
  name: String
}

# query
{
  human(id: 1002) {
    name
    appearsIn
    starships {
      name
    }
  }
}
```

Each field on each type is backed by a function called the **resolver** which is provided by the GraphQL server developer. When a field is executed, the corresponding resolver is called to produce the next value.

> **Note** If a field produces a scalar value like a string or number, then the execution completes. However if a field produces an object value then the query will contain another selection of fields which apply to that object. This continues until scalar values are reached. GraphQL queries always end at scalar values.

## Resolvers

At the top level of every GraphQL server is a type that represents all of the possible entry points into the GraphQL API, it's often called the **Root type** or the **Query type**.

In the example above, our Query type provides a field called `human` which accepts the argument `id`. The resolver function for this field likely accesses a database and then constructs and returns a `Human` object.

```gql
# Asynchronous resolver
Query: {
  human(obj, args, context, info) {
    return context.db.loadHumanByID(args.id).then(
      userData => new Human(userData)
    )
  }
}

# Trivial resolver
# Now that a Human object is available, GraphQL execution
# can continue with the fields requested on it.
Human: {
  name(obj, args, context, info) {
    return obj.name
  }
}
```

- `obj` The previous object, which for a field on the root Query type is often not used.
- `args` The arguments provided to the field in the GraphQL query.
- `context` A value which is provided to every resolver and holds important contextual information like the currently logged in user, or access to a database.
- `info` A value which holds field-specific information relevant to the current query as well as the schema details, also  [refer type GraphQLResolveInfo for more details](http://graphql.github.io/graphql-js/type/#graphqlobjecttype).
