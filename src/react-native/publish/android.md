# Understand concepts

## Version code

Used internally by google play store and not visible to user.

> Integer. Nothing other than 0-9

> **Warning: **The greatest value Google Play allows for `versionCode` is 2100000000.

```bash
# Previous version code
android:versionCode="1"

# Next must be
android:versionCode="2"

# Incorrect
# You cannot use semnatic versioning
android:versionCode="1.0.1"
```

## Version name

Visible to users.

> Keep it sync with RN `package.json`

When publishing new and updated release.

```bash
# You can use semantic versioning
android:versionName="1.0.1"
```

### How to use version code and version name together

1. Google play store always wants new `version code` for updated release.
2. It is not mandatory to change `version name`. But it is good practice to change it so that users can know about updated version and what changes it brings.
3. Sometimes it happens that when you release the app and later find out that you have missed to update icon or some other feature then you can keep `version name` same but change the `version code`.

## Check things before publishing release

> Check the following by installing released (not a debug/development) version in both Emulator and Physical device.

### Package name

**Before releasing app for first time.**

You must choose an appropriate package name for your app because once published to store then you cannot change that because package name becomes a unique id of your app.

```

# React Native by default creates package name, say for example you
# have app name MudraConvert then package name would be

com.mudraconvert

# change it to more appropriate

me.abhaydgarg.mudraconvert
```

### Version code and version name

React native by default set version code to `1` and version name to `1.0`.

Use `1.0.0` version name, when release first time and use semantic versioning later on for each update and keep it sync with `package.json`.

### Check permissions

RN by default set unneccessary permissions which you must remove before publishing app.

Also check the permissions, app going to need by installing production apk to emulator.
[https://medium.com/@applification/fixing-react-native-android-permissions-9e78996e9865](https://medium.com/@applification/fixing-react-native-android-permissions-9e78996e9865)

> Do not remove `SYSTEM_ALERT_WINDOW` permission.

### Features

Thoroughly check bug fixes, updated feature and new feature before actually uploading release to store.
