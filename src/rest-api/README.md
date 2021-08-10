# REST API

## Versioning

Version API.

## Resource archetypes

### Document

A document resource is a singular concept that is akin to an object instance or database record.

!!! important
    A **singular noun** should be used for document names

    `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/claudio`

### Collection

A collection resource is a server-managed directory of resources.

!!! important
    A **plural noun** should be used for collection names

    `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players`

## Use nouns instead of verbs

!!! important
    All url which deal with books below are same and their action is identified by HTTP request method.

!!! example "Correct way"
    - GET /books/123
    - DELETE /books/123
    - POST /books
    - PUT /books/123

!!! example "Wrong way"
    - GET /addBooks/123
    - GET /DeleteBooks/123
    - POST /DeleteAllBooks
    - POST /books/123/delete

## CRUD function names should not be used in URIs

HTTP request methods should be used to indicate which CRUD function is performed.

!!! example
    DELETE /users/1234

    **Wrong:** /users/1234/delete

## Use resource nesting to show relations or hierarchy

For example in the online store, we have ‘users’ and ‘orders’. Orders always belong to some user, therefore we may have the following endpoints structure laid out:

!!! example
    - /users - user’s list
    - /users/123 - specific user
    - /users/123/orders - orders list that belongs to a specific user
    - /users/123/orders/0001 - specific order of a specific user

## A trailing forward slash (/) should not be included in URIs

!!! example
    - **Correct:** http://api.canvas.restapi.org/shapes
    - **Wrong:** http://api.canvas.restapi.org/shapes/

## Lowercase letters should be preferred in URI paths

## The query component of a URI may be used to filter collections or stores

!!! example
    GET /users?role=admin

## Request Methods

- GET must be used to retrieve a resource.
- HEAD should be used to retrieve response headers.
- PUT must be used to update whole resource.
- PATCH must be used to partially update resource.
- POST must be used to create a new resource.
- DELETE must be used to remove a resource.

## Response status codes

- **1xx: Informational** - Communicates transfer protocol-level information.
- **2xx: Success** - Indicates that the client’s request was accepted successfully.
- **3xx: Redirection** - Indicates that the client must take some additional action in order to complete their request.
- **4xx: Client Error** - This category of error status codes points the finger at clients.
- **5xx: Server Error** - The server takes responsibility for these error status codes.

## JSON must be well-formed

[Guidelines](https://abhaydgarg.github.io/Notebook/javascript/json/)

## HTTP Headers

- Content-Type must be used.
- Content-Length should be used - Two reasons. First, a client can know whether it has read the correct number of bytes from the connection. Second, a client can make a HEAD request to find out how large the entity-body is, without downloading it.
- Last-Modified should be used in responses -  Clients and cache intermediaries may rely on header to determine the freshness of their local copies of a resource’s state representation.
- ETag should be used in responses.
- Cache-Control, Expires, and Date response headers should be used to encourage caching.

## Use standard HTTP error code to inform client about error

Always send already existed http error to client. For example, validation failed can be sent with status code Bad Request (400) and say during establishing DB connection or reading file.

If any error occurred then you should never send error message to client that DB connection is failed. Just send http error 500 to client. And internally you can use watchdog that will log errors to DB or file for developer to check.

### Error Response

> Must have uniform error model/schema.

```js
{
  statusCode: "number"   // the http status code
  error: "string"        // the http error message
  message: "string"      // the user error message
}

// Example.
{
  statusCode: 400
  error: "Bad Request"
  message: "body.id is required parameter."
}
```

## What should be the response's data and HTTP status code?

There are 3 options that you can choose. All options have their own benefits and drawbacks. That you can easily undesratnd when you go though each options.

1. For `create` and `update`, API can send whole record back in response. This involves an extra job for API to prepare record after request get successful. Also, the logic that is used in a `GET` for single record has to be re-used in `create` and `update`. So you have to ensure that the logic should not be repeated in multiple places. This is the thing that need to take care on this approach. For `delete`, API can simply send the ID that was deleted in response's body.
2. For `create`, `update`, and `delete`, API can either send a ID in a response's body or in a custom header. And let the API consumer to decide to use ID to fetch fresh record or leave it.
3. API does not send any content back.

> In my opinion, the easies approach for API with less/simpler code is sending ID either in response body or custom header.

!!! info "Custom Header Naming"
    - Use of `X-` has been deprecated. But you can still use it.
    - Must be lower-case and separated by `-`.

    **Reccommendation:** Use name without `-X` that reflects the purpose clearly.

!!! note "node.js"
    Headers are case-insensitive. But node.js web server explicitly converts them to lower-case. That's because Javascript is case-sensitive, so to simplify things they normalize all headers to lower-case.

### GET: 200

### POST: 201

```
# Set location header of created resource.
Location: /v1/items/12

# Response
1. Newly created record data | record ID in response's body.
2. Record ID in custom header.
3. no-content.
```

### PUT: 200 | 204

> 204 - when no content is send back in reponse.

```
# Response
1. Updated record data | record ID in response's body.
2. Record ID in custom header.
3. no-content.
```

### DELETE: 200 | 204

> 204 - when no content is send back in reponse.

```
# Response
1. Record ID in response's body.
2. Record ID in custom header.
3. no-content.
```

## PUT vs PATCH

PATCH must be used when you want to just update the record partially. And PUT must be used when you want to update the record fully. That is in request body you have to send all fields where as in PUT you send a partial fields in request body.

PUT has a advantage that you can reuse the POST validations and request handler logic with less modifications. This makes the code easy to handle and maintain. Where as in PATCH, we can also reuse the POST validations and logic but when things get started little complex, PATCH logic also get started complex. Because in some scenarios, some fields are dependent upon others for running validations and logic also depends upon analysing more than 1 fields. Also, it becomes complex to handle required fields because in PATCH, required fields become optional. Take away is that PATCH is comlex to handle than PUT.

We can choose to have PUT for updating the record and in special cases PATCH can be used separately along with PUT.

If the important requirement is to save the bandwidth by reducing the amount of data to be sent in request body. Then you can choose PATCH over PUT for updting record. But build the solid strategy in how to handle PATCH complexity efficiently.

## What is best/worst range for response time?

1. **BEST** = 0.1 sec - 0.4 sec (100 ms - 400 ms)
2. **OK** = 0.5 sec - 1 sec (500 ms - 1000 ms). There is a room for improvement.
3. **WORST** = Greater than 1.0 sec (1000 ms).

## Others

## Not found

It should also be used for resource that is not found that is endpoint is correct but record is not in DB (`posts/1` - post with id 1 is not found).

### Params model for function that consume API

Params should be passed as object `params = {}` for all functions that consume API, so that we can add remove the fields without changing the function signature. Does not matter if there is only 1 field. Keep it uniform across all consumer functions.

```js
params = {
  id: 123,
  catId: 'women'
  query: {
    sort: 'asc'
  }
}
```

- `id` and `catId` are path paramteres that go into for example, `GET app/:id/categories/:catID`.
- `query` property contains the query parameters that build query string after `?`.
