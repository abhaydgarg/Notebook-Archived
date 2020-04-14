# Understand concepts

## Version Number

Keep it sync with `package.json`.

Make it same as version number in android, if app is also released in google play store.

## Build Number

You can use it as `Version Code` in android.

Android only allowed `integer` in version code but ios allowed any character and you can use decimal etc. But just to keep it simple use `integer` in ios too and increment by 1 always.

When submitting a new release of your app to the App Store, it is normal to have some false starts. You may forget an icon in one build, or perhaps there is a problem in another build. As a result, you may produce many builds during the process of submitting a new release of your app to the App Store. Because these builds will be for the same release of your app, they will all have the same version number. But, each of these builds must have a unique build number associated with it so it can be differentiated from the other builds you have submitted for the release. The collection of all of the builds submitted for a particular version is referred to as the 'release train' for that version.

### How these numbers work together

In android, both do not work together but separately. Google play store just need `version code` to be different in order to upload and use release.

On other hand, Apple Store, uses `version number` and `build number` together to identify release.

Say you have app ready to be released and initial version would be `1.0.0`.

```
# First time release
version number - 1.0.0
build number - 1

# found that updated icon is missed in release
# no need to update the version because nothing
# change in code
version number - 1.0.0
build number - 2

# again change something even in code or feature
# but you want to keep version name same
version number - 1.0.0
build number - 3

# later updated version number
version number - 1.0.1
build number - 1

# updated version to major release
version number - 2.0.0
build number - 1
```

So in Apple store, combination of both make unique identifier that identify the released package. You can re-use build number for different version numbers. Fo example, for version number `1.0.0` you can start the build number from 1 to infinity. And when release different version say `1.0.1` you can re-use the build number again start from 1 to infinity.

On the other hand, in Google Play Store, `version code` must always be unique irrespective of version number. You can keep version number same for each release but version code must be unique.

## Check things before publishing release

> Check the following by installing released (not a debug/development) version in both Emulator and Physical device.

### Package name

**Before releasing app for first time.**

You must choose an appropriate package name for your app because once published to store then you cannot change that because package name becomes a unique id of your app.

```
# React Native by default creates package name, say for example
# you have app name MudraConvert then package name would be

com.mudraconvert

# change it to more appropriate

me.abhaydgarg.mudraconvert
```

### Version code and version name

React native by default set version code to `1` and version name to `1.0`.

Use `1.0.0` version name, when release first time and use semantic versioning later on for each update and keep it sync with `package.json`.

### Check permissions

RN by default set unneccessary permissions which you must remove before publishing app.

### Features

Thoroughly check bug fixes, updated feature and new feature before actually uploading release to store.
