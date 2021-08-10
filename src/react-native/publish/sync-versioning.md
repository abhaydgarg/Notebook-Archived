# How to sync

## Versioning in package.json, ios and android

* Always keep version number in `package.json`, `ios` and `android` same.
* There is no need to have same `version code` and `build number`. Just version number.

## Changes specific to android only

* Do not update version number
* Increment version code and release the app

## Changes specific to ios only

* Do not update version number
* Increment build number and release the app

## Exception

Say if changes specific to `ios` but you still need to change the `version number`. Then change the version number in package.json, ios and android. And then:

1. Upload package at Apple store and specify the changes made.
2. Upload package at Google play store and for `What is new`, just say `minor fixes`.
