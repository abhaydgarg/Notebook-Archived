# CLI (react-native)

## Link a package

```bash
react-native link <package-name>
```

Linking automatically add package to android and ios environment which you would have to do manually.

!!! question "Should pod and link both be used for ios together?"
    No, you should not use both. Reccomended way to link package to ios is by `pod install`.

### How to link a package to specific env.

```bash
# android
react-native link <package-name> --platforms android

# ios
react-native link <package-name> --platforms ios
```
