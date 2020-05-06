# More hooks

## useRef

This is the same as `createRef`.

So if both are same then **why `useRef` is existed and why can't be use `createRef` instead?**

Since both serve the same purpose which is to store the reference but `useRef` is there to handle the scenario that emerges in function comp.

When you use `createRef` in class comp, you place it in constructor so that it runs once. But in the function comp you do not have a place like contructor and you have to place it in function body. So placing `createRef` in function comp executes it every time comp update and hence create ref instance each time.

Therefore, `useRef` is created. We can use `const inputRef = useRef();` in function component and React will make sure that it runs once. It takes care of returning the same `ref` each time which is created at the time of initial render.

## useMemo

> Returns memoized value.

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

As you can see, `useMemo` takes in a function and an array dependencies (the array `[a, b]`) that become function arguments too. The dependencies are the elements `useMemo` watches: if there are no changes the function result will stay the same, otherwise it will re-run the function. If they don’t change, it doesn’t matter if our entire component re-renders, the function won’t re-run but instead return the stored result. This can be optimal if the wrapped function is large and expensive. That is the primary use for `useMemo`.

!!! note "Missing dependency vs Empty dependency []"
    - If array is **missing** then there is no possibility of memoization. The function always run.
    - If array is empty `[]` then it runs once.

    Remember: `useEffect` - same mecahnism.

## useCallback

> Returns memoized callback.

```js
const Counter = () => {
  const [count, setCount] = useState(0)
  const [otherCounter, setOtherCounter] = useState(0)

  const increment = () => {
    setCount(count + 1)
  }
  const decrement = () => {
    setCount(count - 1)
  }
  const incrementOtherCounter = () => {
    setOtherCounter(otherCounter + 1)
  }

  return (
    <>
      Count: {count}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={incrementOtherCounter}>incrementOtherCounter</button>
    </>
  )
}
```

The problem here is that any time the counter is updated, all the 3 functions are re-created again.

What should happen instead it’s that if you increment one counter, all functions related to that counter should be re-instantiated. Therefore, in our example above, if we click in either `increment` or `decrement` then only these two function shoud be re-created and `incrementOtherCounter` should not be re-created and vice-versa.

> Now, in most cases this is not a huge problem unless you are passing lots of different functions, all re-created everytime that are proven to be a big cost for your app performance.

If that’s a problem, you can use `useCallback`.

```js
const increment = useCallback(() => {
  setCount(count + 1)
}, [count])
const decrement = useCallback(() => {
  setCount(count - 1)
}, [count])
const incrementOtherCounter = useCallback(() => {
  setOtherCounter(otherCounter + 1)
}, [otherCounter])
```

Make sure you add that array as a second parameter to `useCallback` with the state needed.

Now if you try to click one of the counters, only the functions related to the state that changes are going to be re-instantiated.

!!! note "Missing dependency vs Empty dependency []"
    - If array is **missing** then there is no possibility of memoization. The function always re-created.
    - If array is empty `[]` then it re-creates once.

    Remember: `useEffect` - same mecahnism.
