# Random

## Immer issue - cannot update object in component

When using the redux's state locally in form component. We want to update the local state's data when changed by user through form. Since immer froze the state, therefore, the local copy of state has same restrictions inherited and you cannot directly update the local state. And you have to deep clone the redux's state into local state. Which is not an efficient solution. Rather you must use the `produce` function to handle local state.

```js
import React, { useCallback, useState } from "react";
import produce from "immer";

const TodoList = ({data}) => {
  // `data` contains data from redux store.
  // Which is used as local state thorugh
  // variable `todos`.
  const [todos, setTodos] = useState(data);

  const handleToggle = useCallback((id) => {
    // Instead of deep cloning the data.
    // Use produce function. It allows you
    // to run update in local state (todos)
    // even if the object is frozen and since
    // produce return new state always, therefore
    // the state object from store(data) will be
    // untouched.
    setTodos(
      produce((draft) => {
        const todo = draft.find((todo) => todo.id === id);
        todo.done = !todo.done;
      })
    );
  }, []);

  const handleAdd = useCallback(() => {
    setTodos(
      produce((draft) => {
        draft.push({
          id: "todo_" + Math.random(),
          title: "A new todo",
          done: false
        });
      })
    );
  }, []);

  return (<div>{*/ jsx */}</div>)
}
```
