# Schema

By using the type system, it can be predetermined whether a GraphQL query is valid or not. This allows servers and clients to effectively inform developers when an invalid query has been created, without having to rely on runtime checks.

## !

`!` - field is non-nullable. (Required field). This means that our server always expects to return a non-null value for this field, and if it ends up getting a null value that will actually trigger a GraphQL execution error, letting the client know that something has gone wrong.

## Object types

The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch from your service, and what fields it has.

```gql
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

* `Character`: name of object type.
* `name` and `appearsIn` are fields on the `Character` type. Where `name` is of scalar type called `String` and  `appearsIn` is of some other object type called `Episode`.

## Arguments

Every field on a GraphQL object type can have zero or more arguments, for example the `length` field below has type `LengthUnit`.

```gql
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```

Arguments can be either required or optional. When an argument is optional, we can define a default value - if the unit argument is not passed, it will be set to METER by default.

## Scalar types

* `Int`: A signed 32‐bit integer.
* `Float`: A signed double-precision floating-point value.
* `String`: A UTF‐8 character sequence.
* `Boolean`: `true` or `false`.
* `ID`: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an `ID` signifies that it is not intended to be human‐readable.

> You can specify own custom scalar type, for example `scalar Date`. The you also have to define how that type should be serialized, deserialized, and validated.

## Enumeration types

Special kind of scalar that is restricted to a particular set of allowed values. This allows you to:

1. Validate that any arguments of this type are one of the allowed values.
2. Communicate through the type system that a field will always be one of a finite set of values.

```gql
# This means that wherever we use the type Episode in our schema,
# we expect it to be exactly one of NEWHOPE, EMPIRE, or JEDI.
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

## Lists and !

```gql
# List itself can be null, but it can't have any null members
myField: [String!]

# Examples
myField: null # valid
myField: []  # valid
myField: ['a', 'b'] # valid
myField: ['a', null, 'b'] # error
```

```gql
# list itself cannot be null, but it can contain null values
myField: [String]!

# Examples
myField: null # error
myField: [] # valid
myField: ['a', 'b'] # valid
myField: ['a', null, 'b'] # valid
```

## Interfaces

 An Interface is an abstract type that includes a certain set of fields that a type must include to implement the interface.

```gql
# interface Character that represents any character
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}

# any type that implements Character needs to have these exact fields,
# with these arguments and return types.
type Human implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  starships: [Starship]
  totalCredits: Int
}

type Droid implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  primaryFunction: String
}

# You can see that both of these types have all of the fields from
# the Character interface, but also bring in extra fields, totalCredits,
# starships and primaryFunction, that are specific to that particular type of character.
```

## Union types

```gql
# Wherever we return a SearchResult type in our schema,
# we might get a Human, a Droid, or a Starship.
union SearchResult = Human | Droid | Starship
```

## Input types

In the case of mutations, where you might want to pass in a whole object to be created. In the GraphQL schema language, input types look exactly the same as regular object types, but with the keyword `input` instead of `type`:

```gql
# type definition
input ReviewInput {
  stars: Int!
  commentary: String
}

# mutation
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

**Note:** You can't mix input and output types in your schema. Input object types also can't have arguments on their fields.
