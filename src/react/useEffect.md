# useEffect

> Handles side-effects, that is why it is named useEffect. Effect short for side-effect.

!!! info "Equivalent to"
    useEffect is equivalent to `componentDidMount`, `componentDidUpdate` and `componentWillUnmount` combined.

## It runs both after the first render and after every subsequent render

!!! important
    **AFTER** render - always. Therefore, DOM is available to use.

This is because it combination of `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`. Instead of thinking in terms of “mounting” and “updating”, you might find it easier to think that **effects happen _after render_**.

For example, let say you have a prop called `title` which your component uses for setting the title of HTML doc.

```js
function Example(props) {

  useEffect(() => {
    document.title = props.title;
  });

  return (
    <div>
      // ...
    </div>
  );
}
```

On **first** render comp set the HTML title. Now if it did not run when prop `title` get updated then the HTML doc title would also not get updated. Therefore, it runs after every render, so that changes can be applied when prop `title` get updated.

## You can control if useEffect should run on subsequent render

!!! note "Optimizing Performance by Skipping Effects"
    useEffect always run once, which is after first render that you cannot stop.

    But you can skip useEffect on subsequent render.

### Missing dependency `[]`

This effect runs after every render.

```js
useEffect(() => {
  document.title = props.title;
});
```

### Empty dependency `[]`

If you supply empty dependency, which is an array, then React compare the sameness of previous array and current array. If they differ then React run the `useEffect` otherwise it won't.

```js
useEffect(() => {
  document.title = props.title;
}, [props.title]);
```

!!! note
    You can have multiple props and state variables in an array.

    For example, `[props.title, count, props.id]`

If `props.title` previous value is equal to current value of `props.title` then React will not run the effect. If they are differ then React run the effect, so that HTML title can be updated.

#### Trick to make useEffect behave as componentDidMount

> You can make useEffect run after first render and then skip it for a good.

```js
useEffect(() => {
  document.title = props.title;
}, []);
```

`[]` empty array in a dependency, means that it will be always the same. So during ths sameness test React will see `[] === []` is `true`. And it skip the effect after every render.

## Multiple useEffect to have a separation of concern

You should use multiple useEffect in function comp in order to separate different logic one from another and place them in their own useEffect.

> In other words, useEffect should follow the design rule called "Single Responsibility".

```js
function FriendStatusWithCounter(props) {
  // Separate effect to handle the HTML
  // doc title only.
  useEffect(() => {
    document.title = props.title;
  });

  // Seaprate effect to subscribe and unsubscribe
  // firend status only.
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  // ...
}
```

## Cleanup work in useEffect

There are situation when you need to unsubscribe to event, or something before component umount in order to free up the memory. This can be done in useEffect by supplying the `return` statement.

```js
useEffect(() => {
  // Subscribe.
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

  // Unsubscribe.
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```

The benefit of this is that we do not have to write the subscribe login in `componentDidMount` and unsubscribe login in `componentWillUnMount` methods. At one place we have the same logic handling.

## How useEffect works

After every render - React remove the previous effect and add the current effect. While removing the previous effect React also run the cleanup work if any.

```js
useEffect(() => {
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
```

```
// Mount with { friend: { id: 100 } } props
ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // Run first effect

// Update with { friend: { id: 200 } } props
ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // Clean up previous effect
ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // Run next effect

// Update with { friend: { id: 300 } } props
ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // Clean up previous effect
ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // Run next effect

// Unmount
ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // Clean up last effect
```

### Lets see how verbose this is in class comp

```js
componentDidMount() {
  ChatAPI.subscribeToFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}

componentDidUpdate(prevProps) {
  // Unsubscribe from the previous friend.id
  ChatAPI.unsubscribeFromFriendStatus(
    prevProps.friend.id,
    this.handleStatusChange
  );
  // Subscribe to the next friend.id
  ChatAPI.subscribeToFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}

componentWillUnmount() {
  ChatAPI.unsubscribeFromFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}
```

Forgetting to handle `componentDidUpdate` properly is a common source of bugs in React applications.
