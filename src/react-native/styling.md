# Styling

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
