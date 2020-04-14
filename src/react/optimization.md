# Optimization

## Avoid reconciliation

As we know, reconciliation is a process where react decides if DOM should be updated or not. See it is a next step when react update DOM. But before that it has to decide if update should be done or not. We can avoid this check by using `shouldComponentUpdate` method or extend from `React.PureComponent`.

Here’s a subtree of components. For each one, `SCU` indicates what `shouldComponentUpdate` returned, and `vDOMEq` indicates whether the rendered React elements were equivalent. Finally, the circle’s color indicates whether the component had to be reconciled or not.

![shouldComponentUpdate](./assets/should-component-update-5ee1bdf4779af06072a17b7a0654f6db-9a3ff.png)

Since  `shouldComponentUpdate`  returned  `false`  for the subtree rooted at C2, React did not attempt to render C2, and thus didn’t even have to invoke  `shouldComponentUpdate`  on C4 and C5.

For C1 and C3,  `shouldComponentUpdate`  returned  `true`, so React had to go down to the leaves and check them. For C6  `shouldComponentUpdate`  returned  `true`, and since the rendered elements weren’t equivalent React had to update the DOM.

The last interesting case is C8. React had to render this component, but since the React elements it returned were equal to the previously rendered ones, it didn’t have to update the DOM.

Note that React only had to do DOM mutations for C6, which was inevitable. For C8, it bailed out by comparing the rendered React elements, and for C2’s subtree and C7, it didn’t even have to compare the elements as we bailed out on  `shouldComponentUpdate`, and  `render`  was not called.

For example, if the only way your component ever changes is when the `props.color` or the `state.count` variable changes, you could have shouldComponentUpdate check that:

```js
shouldComponentUpdate(nextProps, nextState) {
  if (this.props.color !== nextProps.color) {
    return true;
  }
  if (this.state.count !== nextState.count) {
    return true;
  }
  return false;
}
```

**OR - **use `React.PureComponent`

The problem is that `PureComponent` will do a shallow comparison between the old and new values. That means it only checks if reference has changed. It does not go through all the values inside the object.
If you have array, you push an item to an array. When react do the shallow comparison it will find no change because reference to old array and reference to updated array are same. This can be avoided by using **immutable** array.

You do not mutate an array but create new array with updated values. This way react will know that array has updated, even in shallow comparison, and trigger the DOM update.

## Using immutable data

As you know, data types such as object and array are reference type. Let’s say we write `shouldComponentUpdate` and are checking user's object `nextState` against `this.state` to make sure that we only re-render components when changes happen in the user's object.

```js
shouldComponentUpdate(nextProps, nextState) {
  if (this.state.users !== nextState.users) {
    return true;
  }
  return false;
}
```

Even if changes happen in the user's object like change of user's name property, React won’t re-render the UI as it’s the same reference.

You can avoid mutation of existing user's object and return new updated user's object.

```js
this.setState(state => ({
  users: {
    ...state.users,
    name: 'robin',
  })
}));
```

## Multiple chunk files

You can consider having two separate files by separating your vendor, or third-party library code from your application code by taking advantage of SplitChunksPlugin for webpack. You’ll end up with `vendor.bundle.js` and `app.bundle.js`. By splitting your files, your browser caches less frequently and parallel downloads resources to reduce load time wait.

## Using production mode flag in webpack

If you are using webpack 4 as a module bundler for your app, you can consider setting the mode option to production. This basically tells webpack to use the built-in optimizations.

```bash
webpack --mode=production
```

## Dependency optimization

When considering optimizing the application bundle size, it’s worth checking how much code you are actually utilizing from dependencies. For example, you could be using  `Moment.js`  which includes localized files for multi-language support. If you don’t need to support multiple languages, then you can consider using  [moment-locales-webpack-plugin](https://www.npmjs.com/package/moment-locales-webpack-plugin)  to remove unused locales for your final bundle.

Another example is  `loadash`. Let’s say you are only using 20 of the 100+ methods, then having all the extra methods in your final bundle is not optimal. So for this, you can use  [lodash-webpack-plugin](https://github.com/lodash/lodash-webpack-plugin)  to remove unused functions.

Here is an extensive list of  [dependencies](https://github.com/GoogleChromeLabs/webpack-libs-optimizations)  which you can optimize.

## Use React.Fragments to avoid additional HTML wrapper

`React.fragments` lets you group a list of children without adding an extra node.

## Avoid inline function definition in the render function

This will create a new instance of the function on each render. This might create a lot of work for the garbage collector.

```js
// Not recommended
render() {
  return (
    <Comment
      onClick={(e)=>{ this.setState({id: id})}}
      comment={comment}
    />
  );
}
```

```js
// Recommended
onCommentClick = (id) => {
  this.setState({id: id})
}

render() {
  return (
    <Comment
      onClick={this.onCommentClick}
      comment={comment}
    />
  );
}
```

## Avoid using index as key

```js
{
  comments.map((comment, index) => {
    <Comment
      {..comment}
      key={index}
    />
  })
}
```

But using the key as the index can show your app incorrect data as it is being used to identify DOM elements. When you push or remove an item from the list, if the key is the same as before, React assumes that the DOM element represents the same component.

It's always advisable to use a unique property as a key, or if your data doesn't have any unique attributes, then you can think of using the  `shortid module`  which generates a unique key.

```js
import shortid from  'shortid';

{
  comments.map((comment, index) => {
    <Comment
      {..comment}
      key={shortid.generate()}
    />
  })
}
```

In certain cases, it's completely okay to use the index as the key, but only if below condition holds:

- The list and items are static.
- The items in the are never going to be reordered or filtered.

## Using web workers for cpu extensive tasks

Web Workers makes it possible to run a script operation in a web application’s background thread, separate from the main execution thread. By performing the laborious processing in a separate thread, the main thread, which is usually the UI, is able to run without being blocked or slowed down.

In the same execution context, as JavaScript is single threaded, we will need to parallel compute. This can be achieved two ways. The first option is using pseudo-parallelism, which is based on  `setTimeout`  function. The second option is to use Web Workers.

Web Workers works best when executing computation extensive operation since it executes code independent of other scripts in a separate thread in the background. This means it doesn’t affect the page’s performance.

## Virtualize long lists

List virtualization, or windowing, is a technique to improve performance when rendering a long list of data. This technique only renders a small subset of rows at any given time and can dramatically reduce the time it takes to re-render the components, as well as the number of DOM nodes created.

There are some popular React libraries out there, like  [react-window](https://github.com/bvaughn/react-window)  and  [react-virtualized](https://github.com/bvaughn/react-virtualized), which provides several reusable components for displaying lists, grids, and tabular data.


