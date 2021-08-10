# Redux

## Data flow

-   Initial setup:
    -   A Redux store is created using a root reducer function
    -   The store calls the root reducer once, and saves the return value as its initial  `state`
    -   When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render. They also subscribe to any future store updates so they can know if the state has changed.
-   Updates:
    -   Something happens in the app, such as a user clicking a button
    -   The app code dispatches an action to the Redux store, like  `dispatch({type: 'counter/increment'})`
    -   The store runs the reducer function again with the previous  `state`  and the current  `action`, and saves the return value as the new  `state`
    -   The store notifies all parts of the UI that are subscribed that the store has been updated
    -   Each UI component that needs data from the store checks to see if the parts of the state they need have changed.
    -   Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen

## Do I have to deep-clone my state when updating?

No, the rule is: If something inside doesn't change, keep its reference the same. Only make copy of those nested items must be cloned that are going to be changed.

## Reducers must not have side effects

> Reducer must be pure function.

Reducer functions should _only_ depend on their `state` and `action` arguments, and should only calculate and return a new state value based on those arguments. **They must not execute any kind of asynchronous logic (AJAX calls, timeouts, promises), generate random values (`Date.now()`,  `Math.random()`), modify variables outside the reducer, or run other code that affects things outside the scope of the reducer function**.

## Do not put non-serializable values in state

Avoid putting non-serializable values such as `Promises`, `Symbols`, `Maps/Sets`, `functions`, or `class instances` into the Redux store state.
