# More about hooks

## Hook execution order on first render

```js
export default function App() {

  React.useEffect(() => {
    console.log('EFFECT')
  })

  const cb = React.useCallback(() => {
    console.log('CALLBACK')
  })

  const value = React.useMemo(() => {
    console.log('MEMO')
    return 1
  })

  console.log('RENDER')

  return (
    <div className="App">
      <h1>Hello World!</h1>
    </div>
  );
}

// 1. MEMO - execute before first render.
// 2. RENDER
// 3. EFFECT

// useCallback does not run the function. It just retruns the function
// reference. When you do cb() it executes then.
```

## useEffect vs useLayoutEffect

Both are same and serve the same purpose but with one difference.

```js
useEffect(() => {
  // do side effects
  return () => /* cleanup */
}, [dependency]);

useLayoutEffect(() => {
  // do side effects
  return () => /* cleanup */
}, [dependency]);
```

### Difference

#### useEffect runs _asynchronously_

1. You cause a render somehow (change state, or the parent re-renders)
2. React renders your component (calls it)
3. The screen is visually updated (DOM mutation)
4. THEN `useEffect` runs

> It does not block browser painting. This differs from the behavior in class components where `componentDidMount` and `componentDidUpdate` run synchronously after rendering. `useEffect` is more performant and most of the time this is what you want.

#### useLayoutEffect runs _synchronously_

1. You cause a render somehow (change state, or the parent re-renders)
2. React renders your component (calls it)
3. `useLayoutEffect` runs, and React waits for it to finish
4. The screen is visually updated (DOM mutation)

##### When to useLayoutEffect

However, if your effect is mutating the DOM (via a DOM node ref) and the DOM mutation will change the appearance of the DOM node between the time that it is rendered and your effect mutates it, then you don't want to use `useEffect`. You'll want to use `useLayoutEffect`. Otherwise the user could see a flicker when your DOM mutations take effect. This is pretty much the only time you want to avoid `useEffect` and use `useLayoutEffect` instead.

This can be useful if you need to make DOM measurements (like getting the scroll position or other styles for an element) and then make DOM mutations or trigger a synchronous re-render by updating state.
