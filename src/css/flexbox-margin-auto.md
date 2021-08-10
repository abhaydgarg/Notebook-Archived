# The peculiar magic of flexbox and auto margins

If you apply auto margins to a flex item, that item will automatically extend its specified margin to occupy the extra space in the flex container, depending on the direction in which the auto-margin is applied.

Let's pick that apart a bit and say we have a simple parent `div` with a child `div` inside it:

```html
<div class="parent">
  <div class="child"></div>
</div>
```

```css
.parent {
  display: flex;
  height: 400px;
  background-color: #222;
}

.child {
  background-color: red;
  width: 100px;
  height: 100px;
}
```

The result is something like this:

![flexbox-margin-auto1](assets/flexbox-margin-auto1.png)

When we add `margin-left: auto` to the `.child` element like so:

```css
.child {
  background-color: red;
  width: 100px;
  height: 100px;
  margin-left: auto;
}
```

...then we’ll see this instead:

![flexbox-margin-auto2](assets/flexbox-margin-auto2.png)

Weird, huh? The left-hand margin is pushing the parent so that the child is nestled up in the far right-hand corner. But it gets a little weirder when we set all margins to `auto`:

```css
.child {
  background-color: red;
  width: 100px;
  height: 100px;
  margin: auto;
}
```

![flexbox-margin-auto3](assets/flexbox-margin-auto3.png)

It’s like we're using a popular centering trick by setting `justify-content` and `align-items` to center because the child decides to rest in the center of the parent, both horizontally and vertically. Likewise, if we set `margin-left` and `margin-top` to `auto`, then we can let the flex item push itself into the `bottom-right` of the parent:

![flexbox-margin-auto4](assets/flexbox-margin-auto4.png)

> Setting the `margin` property on a flex child will push the child away from that direction. Set `margin-left` to `auto`, the child will push right. Set `margin-top` to `auto` and the child will push to the bottom.
