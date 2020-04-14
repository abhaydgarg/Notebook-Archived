# Points to note

## Custom fonts

If you do not have bold file and you set `fontWeight` to `bold` then RN ignore the `fontFamily` and uses a default system font for bold.

## StyleSheet vs Object

The advantanges of `StyleSheet.create` should be the following:

- It validates the styles at the compile time and avoid styles error at the runtime.
- Better performance because it creates a mapping of the styles to an `ID`, and then it refers to `ID`, instead of creating new object every time when component update.
- It also allows to send the style only once through the bridge. All subsequent uses are going to refer an `id`.
