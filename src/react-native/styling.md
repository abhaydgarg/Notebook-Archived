# Styling

## Padding and margin in layout

Container view must have padding instead of margin because, say you set padding `10` for container view. Now any child view does not need to have padding or margin set and your content in a child view will be have space around it automatically. This way you can avoid the style **boilerplate** in a sense that padding is set in container view once and all the child view who will use the same container have the uniform space around which is `10` and you need not to give every child view a padding or margin of value `10` to have uniform spacing.

### Why padding in container view not a margin

Sometimes, you want to ignore the container padding say `10` and have the boundry of child touch with container view because you want to have bottom border goes all the way to container view. Since, we have padding set to container view, we can set the child view margin equal to `-10` (same as container padding). **Cancelling the padding with negative margin**. So we can ignore container view padding through child view negative margin. **This is why container view should have padding not margin.** Note: child view cannot cross the boundry of container view.

Also, say you have a container view padding `10` set, and you want child view has a space `15` for around its content. Then in this case, you know that container view already has a `10` padding and then for child view you can set padding or margin `5` and as result you will have `15` units of space around the child view's content. Or you can cancel the container padding by child negative margin and set padding `15` for child view.

!!! info "Convention"
    Reusable component for layout such as, `ScreenView`, `ModalContent`, `HeaderContent` then:

    1. Set the padding say 10.
    2. Child view reuse it and you do not need to set the padding 10 everytime when the parent comp is reused.
    3. Do not want parent comp padding then you can simply cancel it by setting negative margin in child view.

## Custom fonts

If you do not have bold file and you set `fontWeight` to `bold` then RN ignore the `fontFamily` and uses a default system font for bold.

## StyleSheet vs Object

The advantanges of `StyleSheet.create` should be the following:

- It validates the styles at the compile time and avoid styles error at the runtime.
- Better performance because it creates a mapping of the styles to an `ID`, and then it refers to `ID`, instead of creating new object every time when component update.
- It also allows to send the style only once through the bridge. All subsequent uses are going to refer an `id`.

## Themes folder

## Group globally and common styles and bundle them up in an index.js file

```
- Themes
  - index.js
  - Colors.js
  - Fonts.js
  - Spacing.js
  - Typography.js
  - Buttons.js
```

```js
//# index.js
import Colors from './Colors';
import Fonts from './Fonts';
import Spacing from './Spacing';
import Typography from './Typography';
import Buttons from './Buttons';

export { Colors, Fonts, Spacing, Typography, Buttons };
```

```js
//# Some component
import { Typography, Colors, Spacing } from '../styles';

const styles = StyleSheet.create({
  container: {
    backgroundColor: Colors.background,
    alignItems: 'center',
    padding: Spacing.base,
  },
  header: {
    flex: 1,
    ...Typography.mainHeader,
  },
  section: {
    flex: 3,
    ...Typography.section,
  }
})
```

### Advantages

- Import from the same file every time.
- Import only what we need.
- Easily extend and modify the common styles.

## Use of object destructuring in style

```js
//# Buttons.js
export const small = {
  paddingHorizontal: 10,
  paddingVertical: 12,
  width: 75
};

export const rounded = {
  borderRadius: 50
};

export const smallRounded = {
  ...base,
  ...small,
  ...rounded
};
```

```js
//# Some component
const styles = StyleSheet.create({
  button: {
    ...Buttons.smallRounded,
  },
});
```

is easier to understand the intent of and maintain than:

```js
const styles = StyleSheet.create({
  button: {
    paddingHorizontal: 10,
    paddingVertical: 12,
    width: 75,
    borderRadius: 50
  },
});
```
