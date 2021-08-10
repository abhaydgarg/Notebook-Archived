# Code splitting

Development, is usually done on above-average machines connected to a strong network. However, not all users access the web from a powerful device or a strong signal. What we want to achieve is to not let the user think that our app is loading.

> To make sure that our web app is performant we need to program for the worst, not the best case.

## The cost of javascript

1. The first is the cost to send the code to the user’s browser. The smaller the files we send the faster the user’s browser will receive them.
2. The second cost is the action that the browser needs to take to parse and execute the JavaScript we’ve sent him.

## The concept of laziness

When we talk about performance we need to first understand the concept of laziness.

> The concept is that if we can do something later we never want to do it now. Translated this means that if we can postpone sending particular resources to the browser, we should always do that.

## Dynamic import()

It loads the module and returns a promise that resolves into a module object that contains all its exports. It can be called from any place in the code.

```js
import(modulePath).then((data) => {
  console.log(data);
});

let module = await import(modulePath)

let {foo, bar} = await import(modulePath);
```

## Understanding chunks

It’s not a very good idea to have one big fat file doing everything. Hence, we have the following use cases where we might need to break down `main.js` by extracting chunks from it.

- **Vendor chunk:** Create separate files for vendor code (_3rd party code coming from_ `_node_modules_`). A single `vendor.js` file will suffice. Any vendor code used inside `index.js` (`_import_` _statements of_ `_npm_` _modules_) will break from it and form `vendor.js` which will be loaded synchronously with `main.js`.
- **Async chunks:** Create separate files for code which can be lazy-loaded. Like for example a file for every Route of React router which can be lazy-loaded when a route is changed.
- **Common Chunk:** Create a common file from code which is shared between different chunks. For example, if **10 routes** create **10 different async chunks** and these chunks have a common `import` statement, then code associated with that `import` statement will get injected into respective chunk files separately. That would load the same piece of code every single time we change the route, which is not very good for UX. Instead, we could take out common code shared between different chunks and create `common.js` file which will be loaded synchronously with `main.js` beforehand.

## How WebPack split the code in chunks

Whenever Webpack sees `import()` function, it is going to create an async chunk file. It does not include the module in a `main.js` file and create a separate file in output folder. If you look at the output folder, you will see other js files along with `main.js` file.

## React.lazy and Suspense

The `React.lazy` function lets you render a dynamic import as a regular component. This will automatically load the bundle containing the  `OtherComponent`  when this component is first rendered. `React.lazy`  takes a function that must call a dynamic  `import()`.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

The lazy component should then be rendered inside a `Suspense` component, which allows us to show some fallback content (such as a loading indicator) while we’re waiting for the lazy component to load.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

!!! note "Caveat"
    `React.lazy` currently only supports default exports. If the module you want to import uses named exports, you can create an intermediate module that reexports it as the default.

### About Suspense

- You can wrap multiple lazy components with a single `Suspense` component. But it is important to know that if you do so then a fallback loader indicator goes away after all the lazy components are loaded. For example, loader goes away when `OtherComponent` and `AnotherComponent` are loaded.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

- If you want to have separate fallback (loader) for each lazy component then you can wrap each lazy component in their own `Suspense` component.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
      <Suspense fallback={<div>Loading...</div>}>
        <AnotherComponent />
      </Suspense>
    </div>
  );
}
```

### Error boundaries

If the other module fails to load (for example, due to network failure), it will trigger an error. You can handle these errors to show a nice user experience and manage recovery with Error Boundaries.

```js
import MyErrorBoundary from './MyErrorBoundary';
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <MyErrorBoundary>
        <Suspense fallback={<div>Loading...</div>}>
          <OtherComponent />
        </Suspense>
      </MyErrorBoundary>
    </div>
  );
}
```

## Two types of code splitting

### Split at the route level

Deciding where in your app to introduce code splitting can be a bit tricky. You want to make sure you choose places that will split bundles evenly, but won’t disrupt the user experience. A good place to start is with routes. This will leave us with a separate bundle for each top level route.

```js
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

### Split at the component level

You do not want to load `Comments` component when application starts. It should load when `Movie` component loads. So you make it lazy component. `Comments` module chunk is loaded by Webpack for you and show the content as soon as parsing is done.

> You can also make the `Comments` component loads after user click on say "View Comments" button.

```js
import React, { Suspense } from 'react';
import Description from './Description';

const Comments = React.lazy(() => import('./Comments'));

function Movie() {
  return (
    <div>
      <h1>My Movie</h1>
      <Description />
      <Suspense fallback="Loading...">
        <Comments />
      </Suspense>
    </div>
  );
}
```

## Webpack and import()

### Naming bundles

Webpack keeps track of chunks by giving each one an id. So when you fetch a dynamically loaded bundle you will most likely see a file with a name similar to `1.bundle.js` loaded in the Developer Tools.

However, by using Webpack’s magic comments and making a small change in the configuration we can give the different chunks more descriptive names.

```js
import(/* webpackChunkName: 'Home' */ './Home');
```

### Preloading and prefetching

Preloading and prefetching are two techniques that we can use is addition to code splitting to further improve our performance.

The importance of component level splitting when some of them use expensive libraries. However, loading the chunk when the user presses a button is still not ideal, for it could lead to flashes of empty content or a short freeze of the UI before what we need is rendered.

Whenever we are confident that the user will need a particular bundle, we can use preloading or prefetching to pull it before it is explicitly required. This is again achieved with a Webpack magic comment.

```js
import(/* webpackPrefetch: 'Home' */ './Home');
import(/* webpackPreload: 'Blog' */ './Blog');
```

While both preloading and prefetching will fetch a chunk before it is actually required, they will do it with a different level of importance.

**Preloaded** chunks will be loaded with higher priority in parallel to its parent chunk. Mark chunks to be preloaded only if you are confident that the user **will** interact with them **immediately**.

**Prefetched** chunks have lower priority and will be loaded in the browser’s idle time. In other words, mark chunks to be prefetched if the user **may** need them at some point.

## Common chunk

Let's say we have two lazy components `Home` and `About` and both import component `Util`. So when, webpack creates chunk file of `Home` and `About` then it imcludes the code of `Util` component in both lazy components.

We could avoid this, if we create `common.js` where if a module is shared between different chunks 2 or more times, then form a new chunk.

```js
// vendor chunk
vendor: {
  // name of the chunk
  name: 'vendor',
  // async + async chunks
  chunks: 'all',
  // import file path containing node_modules
  test: /node_modules/,
  // priority
  priority: 20
},
// common chunk
common: {
  name: 'common',
  minChunks: 2,
  chunks: 'async',
  priority: 10,
  reuseExistingChunk: true,
  enforce: true
}
```

`reuseExistingChunk`  tells  `SplitChunksPlugin`  to use existing chunk if available instead of creating a new one. For example, if any module imported inside common chunk code is part of another chunk already, then instead of creating a new chunk for it, the older one is reused instead.

If a module falls under many chunk groups, then the module will be a part of a chunk group with higher  `priority`. For example, if  `common`  chunk has no  `test`  cases and  `chunks`  set to  `all`, that means any imported module (_statically or dynamically_) will be part of  `common`  chunk as well, even  `npm`  module. But since  `vendor`  chunk has higher priority,  `npm`  modules are more likely to be a part of  `vendor`  chunk and reset will stay inside  `common`  chunk.

`enforce`  value is set to true to force  `SplitChunksPlugin`  to form this chunk irrespective of the size of the chunk.  `SplitChunksPlugin`  by default only form chunks if the resulting chunk size is greater than 30kb. In our case,  `common`  chunk contains only  `HelloComponent`  code which is obviously less than 30kb.  `enforce`  is necessary here or set value of [splitChunks.minSize](https://webpack.js.org/plugins/split-chunks-plugin/#splitchunks-minsize) as minimum as possible.

Looks like any imported modules which don’t fall inside  `vendor`  chunk will be a part of  `common`  chunk  **but it can’t**.  `minChunks`  tells  `SplitChunksPlugin`  to only inject module inside  `common`  chunk if and only if they are shared between at least 2 chunks (_sync or async because of all value of chunks_).

