# Redux toolkit

## Slices

**A "slice" is a collection of Redux reducer logic and actions for a single feature in your app**, typically defined together in a single file. The name comes from splitting up the root Redux state object into multiple "slices" of state.

```js
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

Since we know that the `counterReducer` function is coming from `features/counter/counterSlice.js`, let's see what's in that file, piece by piece.

```js
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  // It is act as prefix and create action's type
  // like {type: "counter/increment"}
  // {type: "counter/decrement"}
  name: 'counter',
  initialState: {
    value: 0
  },
  // Reducer functions uses the immer library internally.
  reducers: {
    increment: state => {
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

// createSlice automatically generates action creators
// with the same names as the reducer functions we wrote.
// We can call them as counterSlice.actions.increment().
// This one is imported in a file where we want to
// dispatch the action.
export const { increment, decrement, incrementByAmount } = counterSlice.actions

// This one is imported in file where
// store is created using configureStore().
export default counterSlice.reducer
```

## Payload type

`payload` can be of any type. If you pass `object` in action creator function then it will be object and if you pass `string` then it will be `string`.

```js
postUpdated({ id: '123', title: 'First Post', content: 'Some text here' })
/*
{
  type: 'posts/postUpdated',
  payload: {
    id: '123',
    title: 'First Post',
    content: 'Some text here'
  }
}
*/
```

## createEntityAdapter & normalized state

> Utility API for handling CRUD operations for normalized state.

Redux toolkit recommends to normalize state. But it up to you to evaluate that should state be normalized or not.

- "Normalization" means no duplication of data, and keeping items stored in a lookup table by item ID.
- Normalized state shape usually looks like  `{ids: [], entities: {}}`
- RTK provides `createEntityAdapter` API to helps manage normalized data in a slice. For example, the functions below help us to handle **CRUD** operation on state.

    -   `addOne`  /  `addMany`: add new items to the state
    -   `upsertOne`  /  `upsertMany`: add new items or update existing ones
    -   `updateOne`  /  `updateMany`: update existing items by supplying partial values
    -   `removeOne`  /  `removeMany`: remove items based on IDs
    -   `setAll`: replace all existing items
- `getSelectors()` gives us back set of function to extract data from normalized state such as `selectAll` and `selectById`.
