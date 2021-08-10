# How to structure state & code

## Normalization vs Denormalization

> What is the good approach for state model?

Normalization means we only have one copy of each particular piece of data in our state, so there's no duplication. Whereas, denormalization allows us to have duplicate data in our state.

There is no fixed answer that applies in all scenarios. You have to evaluate whether a normalized or a denormalized state is better for your whole application or we can either normalized or denormalized a part of state (slice).

## Loading state using enum values

```js
{
  status: 'idle' // or: 'loading', 'succeeded', 'failed'
}
```

A boolean limits us to two possibilities only: "loading" or "not loading". In reality,  **it's possible for a request to actually be in  _many_  different states**, such as:

-   Hasn't started at all
-   In progress
-   Succeeded
-   Failed
-   Succeeded, but now back in a situation where we might want to refetch

It is possible that in future or by requirement change, we need to have different state for action. Because of this, we recommend **storing loading state as a string enum value instead of boolean flags**.

## Rules for normalized state

- Each type of data gets its own "table" in the state.
- Each "data table" should store the individual items in an object, with the IDs of the items as keys and the items themselves as the values.
- Any references to individual items should be done by storing the item's ID.
- Arrays of IDs should be used to indicate ordering.

```js
{
  posts: {
    status: 'idle',
    // Sortable ids.
    ids: ["post1", "post2"],
    // Data.
    entities: {
      "post1" : {
          id : "post1",
          author : "user1",
          body : "......",
          // Comment ids reference.
          comments : ["comment1", "comment2"]
      },
      "post2" : {
          id : "post2",
          author : "user2",
          body : "......",
          comments : ["comment3", "comment4", "comment5"]
      }
    }
  },
  comments : {
    ids: ["comment1", "comment2"],
    entities: {
      "comment1" : {
          id : "comment1",
          author : "user2",
          comment : ".....",
      },
      "comment2" : {
          id : "comment2",
          author : "user3",
          comment : ".....",
      },
    }
  }
}
```

### What about relationship between data

Because we're treating a portion of our Redux store as a "database", many of the principles of database design also apply here as well. For example, if we have a many-to-many relationship, we can model that using an intermediate table that stores the IDs of the corresponding items (often known as a "join table" or an "associative table").

```js
{
  authors : { entities : {}, ids : [] },
  books : { entities : {}, ids : [] },
  // Relationship between author & book.
  authorBook : {
    entities : {
        1 : {
            id : 1,
            authorId : 5,
            bookId : 22
        },
        2 : {
            id : 2,
            authorId : 5,
            bookId : 15,
        },
        3 : {
            id : 3,
            authorId : 42,
            bookId : 12
        }
    },
    ids : [1, 2, 3]
  }
}
```

## Lets take an example of posts feature

1. One feature => One file. Have one file for handling feature's state including all, create, update, delete, get and get all.
2. Reducer naming: `actionFeatureState` where `Feature` can be singular or plural. For example, `getPostsRequest` and `getPostRequest`.
    - actions (prefix): `get`, `create`, `update`, `delete`.
    - feature (middle): Singular or plural like `Posts` and `Post`. For **batch** and **multiple** API requests, prepend `Batch`. For example, `deleteBatchPostRequest`, `createBatchPostRequest`
    - states (suffix): `Request`, `Success`, `Failure`.
3. When running multiple or batch API request. Do not use `Complete` state. Use `Success` and `Failure` only. The logic that you can adopt is to collect the succeded and failed responses and then for succeded responses dispatch a single success action and for failed responses dispatch single failure action. And it is upto reducer how to handle them.

```js
export const postsSlice = createSlice({
  name: 'posts',
  initialState: {
    // Can be used either normalized or denormalized.
    // If using normalized then do use createEntityAdapter.
    // DO NOT USE SEPARATE PROPERTIES LIKE `posts` to store
    // all posts and `post` to store single current post.
    // All posts must be stored under `posts` and add/update/delete
    // operation must be done in `posts` list.

    // idle, 'loading', 'succeeded', 'failed'.
    status: {
      getPosts: 'idle',
      getPost: 'idle',
      createPost: 'idle',
      updatePost: 'idle',
      deletePost: 'idle',
      deleteBatchPost: 'idle',
    },
    // Either null, string, object, array.
    // null = no error.
    error: {
      getPosts: null,
      getPost: null,
      createPost: null,
      updatePost: null,
      deletePost: null,
      deleteBatchPost: null,
    }
  },
  reducers: {
    getPostsRequest: (state, action) => {
      state.status.getPosts = 'loading'
    },
    getPostsSuccess: (state, action) => {
      state.status.getPosts = 'succeeded'
    },
    getPostsFailure: (state, action) => {
      state.status.getPosts = 'failed'
      state.error.getPosts = action.payload
    },
    getPostRequest: (state, action) => {
      state.status.getPost = 'loading'
    },
    getPostSuccess: (state, action) => {
      state.status.getPost = 'succeeded'
    },
    getPostFailure: (state, action) => {
      state.status.getPost = 'failed'
      state.error.getPost = action.payload
    },
    createPostRequest: (state, action) => {
      state.status.createPost = 'loading'
    },
    createPostSuccess: (state, action) => {
      state.status.createPost = 'succeeded'
    },
    createPostFailure: (state, action) => {
      state.status.createPost = 'failed'
      state.error.createPost = action.payload
    },
    updatePostRequest: (state, action) => {
      state.status.updatePost = 'loading'
    },
    updatePostSuccess: (state, action) => {
      state.status.updatePost = 'succeeded'
    },
    updatePostFailure: (state, action) => {
      state.status.updatePost = 'failed'
      state.error.updatePost = action.payload
    },
    deletePostRequest: (state, action) => {
      state.status.deletePost = 'loading'
    },
    deletePostSuccess: (state, action) => {
      state.status.deletePost = 'succeeded'
    },
    deletePostFailure: (state, action) => {
      state.status.deletePost = 'failed'
      state.error.deletePost = action.payload
    },
    deleteBatchPostRequest: (state, action) => {
      state.status.deleteBatchPost = 'loading'
    },
    deleteBatchPostSuccess: (state, action) => {
      state.status.deleteBatchPost = 'succeeded'
    },
    deleteBatchPostFailure: (state, action) => {
      state.status.deleteBatchPost = 'failed'
      state.error.deleteBatchPost = action.payload
    }
  }
})

export const {
  getPostsRequest,
  getPostSuccess,
  getPostFailure,
  // ...
} = postsSlice.actions

export default postsSlice.reducer

export const PostsSelectors = {
  getPosts: (state) => {},
  getPostsStatus: (state) => state.posts.status.getPosts,
  getPostsError: (state) => state.posts.error.getPosts,
  getPost: (state) => {}
  getPostStatus: (state) => state.posts.status.getPost
  getPostError: (state) => state.posts.error.getPost,
  createPostStatus: (state) => state.posts.status.createPost,
  createPostError: (state) => state.posts.error.createPost,
  deleteBatchPostStatus: (state) => state.posts.status.deleteBatchPost,
  deleteBatchPostError: (state) => state.posts.error.deleteBatchPost,
  // ...
}
```
