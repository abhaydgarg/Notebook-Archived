# Flex

## [1] Flexbox works the same way as in CSS with a few exceptions. By **default:**

1. `flexDirection` = column.
2. `flex` only supports a single number and is not same as in CSS.

## [2] View can be nested

## [3] All components have flex property, not just a View even Text too

Try to wrap components inside `View`. Also make a note that `ListView`, `ScrollView`, `KeyboardAvoidingView` etc, the components with `View` as suffix are special kind of components which can be used as topmost container like `View`.

## [4] flexGrow, flexShrink, and flexBasis work the same as in CSS

[Flexbox CSS](/css/flexbox/)

## [5] flex

In React Native `flex` does not work the same way that it does in CSS. `flex` is a number rather than a string `flex: <flex-grow, flex-shrink, flex-basis>` which is a shorthand in a CSS.

### Default: flex = 0

Component size is set by `width` and `height` and it is inflexible.

- **`flex: 0`**
    - Element takes the size of contents. According to the  [documentation](https://facebook.github.io/react-native/docs/layout-props.html#flexgrow)  it should be sized by setting  `width`  and  `height`  props but it seems to fit to contents if those aren't set.
- **`flex: 0, flexBasis: {{px}}`**
    - Element takes the size given by  `flexBasis`
- **`flex: 0, flexGrow: 1`**
    - With  `flex: 0`  and  `flexGrow: 1`; it's the same as adding the size of the contents (in the example above it's a ) to the size of an element that's set to  `flex: 1`. It's similar to  `flex: 1, flexBasis: 10`  except instead of adding a number of pixels you're adding the size of the content.
- **`flex: 0, flexShrink: 1`**
    - With  `flex: 0`  and  `flexShrink: 1`, the element seems to take the size of the content, in other words it's the same as just  `flex: 0`. I'll bet there are situations where it would be bigger than the content but I haven't see that yet.
- **`flex: 0, flexGrow: 1, flexBasis: {{px}}`**
    - This is the same as  `flex: 0, flexGrow: 1`  except instead of adding the content size to a  `flex: 1`  element it adds the given number of pixels.
- **`flex: 0, flexShrink: 1, flexBasis: {{px}}`**
    - This is the same as  `flex: 0, flexBasis: {{px}}`.
- **`flex: 0, height: {{px}}`**
    - With  `flex: 0`,  `height`  is treated just like  `flexBasis`. If there is both a  `height`  and  `flexBasis`  are set,  `height`  is ignored.

### flex >= 1 - Positive number

It makes the component flexible and it will be sized proportional to its `flex` value. So a component with `flex` set to 2 will take twice the space as a component with `flex` set to 1.

- **`flex: 1, flexBasis: {{px}}`**
    - With  `flex: 1`  and  `flexBasis: {{px}}`; the value of  `flexBasis`  is added to the element's size. In other words, it's like taking a  `flex: 1`  element and adding on the number of pixels set by  `flexBasis`. So if a  `flex: 1`  element is 50px, and you add  `flexBasis: 20`  the element will now be 70px.
- **`flex: 1, flexGrow: 1`**
    - ignored
- **`flex: 1, flexShrink: 1`**
    - ignored
- **`flex: 1, flexGrow: 1, flexBasis: {{px}}`**
    - This is the same as  `flex: 1, flexBasis: {{px}}`  since  `flexGrow`  is ignored.
- **`flex: 1, flexShrink: 1, flexBasis: {{px}}`**
    - This is the same as  `flex: 1, flexBasis: {{px}}`  since  `flexShrink`  is ignored.
- **`flex: 1, height: {{px}}`**
    - With  `flex: 1`,  `height`  is ignored. Use  `flexBasis`  instead.

### flex = -1

The component is normally sized according `width` and `height`. However, if there's not enough space, the component will shrink to its `minWidth` and `minHeight`.

#### Observations

- Make sure the parent view(s) are giving the children room to grow/shrink. If you're tryign to figure out why something isn't displaying like you think it should, start with the (most) parent element and make sure it's giving enough space to it's children to do what they need to do. In other words, try setting it to `flex:1` and see if that helps, then go to the next child and repeat.
- Don't use Hot Reloading when testing these values, it can display elements incorrectly after it's reloaded a few times. Use Live Reload.
- The default flex value is flex: 0. If you don't add a flex style value it defaults to 0.
- In general use `flexBasis` over `height` since `flexBasis` trumps `height`.
