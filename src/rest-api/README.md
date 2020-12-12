# REST API

## Versioning our APIs

## Resource Archetypes

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

## Use Nouns instead of Verbs

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

- GET must be used to retrieve a resource
- HEAD should be used to retrieve response headers
- PUT must be used to update resource
- POST must be used to create a new resource
- DELETE must be used to remove a resource

## Response Status Codes

- **1xx: Informational** - Communicates transfer protocol-level information.
- **2xx: Success** - Indicates that the client’s request was accepted successfully.
- **3xx: Redirection** - Indicates that the client must take some additional action in order to complete their request.
- **4xx: Client Error** - This category of error status codes points the finger at clients.
- **5xx: Server Error** - The server takes responsibility for these error status codes.

## Use standard HTTP error code to inform client about error

Always send already existed http error to client. For example, validation failed can be sent with status code Bad Request (400) and say during establishing DB connection or reading file.

If any error occurred then you should never send error message to client that DB connection is failed. Just send http error 500 to client. And internally you can use watchdog that will log errors to DB or file for developer to check.

## JSON must be well-formed

[Guidelines](https://abhaydgarg.github.io/Notebook/javascript/json/)

## HTTP Headers

- Content-Type must be used.
- Content-Length should be used - Two reasons. First, a client can know whether it has read the correct number of bytes from the connection. Second, a client can make a HEAD request to find out how large the entity-body is, without downloading it.
- Last-Modified should be used in responses -  Clients and cache intermediaries may rely on header to determine the freshness of their local copies of a resource’s state representation.
- ETag should be used in responses.
- Cache-Control, Expires, and Date response headers should be used to encourage caching.
