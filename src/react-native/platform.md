# Platform

## Platform specific

### Style _(select)_

```js
const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'green',
      },
      default: {
        // other platforms, web for example
        backgroundColor: 'blue',
      },
    }),
  },
});
```

### Any _(select)_

```js
const height = Platform.OS === 'ios' ? 200 : 100
```

```js
const Component = Platform.select({
  native: () => require('ComponentForNative'),
  default: () => require('ComponentForWeb'),
})();

<Component />;
```

### File selection

When your platform-specific code is more complex, you should consider splitting the code out into separate files. React Native will detect when a file has a `.ios` or `.android`. extension and load the relevant platform file when required from other components.

```
BigButton.ios.js
BigButton.android.js
```

React Native will automatically pick up the right file based on the running platform.

```js
import BigButton from './BigButton';
```
