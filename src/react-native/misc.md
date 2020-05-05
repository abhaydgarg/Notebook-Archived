# Misc

## Apply click event on disabled TextInput

You have `TextInput` comp which is non-editable `(editable={false})`. In this case you cannot perform press event on it because all event are disabled for non-editable `TextInput`. But you want to open a Date modal comp when press non-editable `TextInput`.

```jsx
//# 1 - pointerEvents

<TouchableOpacity onPress={() => console.log("Pressed")}>
  <TextInput
   pointerEvents="none"
   editable={false}
  />
</TouchableOpacity>

//# 2 - onTouchStart (ios only)
<TextInput
    onTouchStart={() => console.log("Hello...")}
    editable={false}
/>
```
