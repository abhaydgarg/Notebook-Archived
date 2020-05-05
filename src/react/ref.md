## ref

!!! info "Same As"
    `document.getElementById` in DOM.

`props` is the reccomended way in React to interact with child comp. You do it by sending the updated `props` to child. But in some cases, you do not to take a help of `props` and access the comp directly and perform certain operation. This is where `ref` comes in to picture.

### Use cases

- Managing focus, text selection, or media playback.
- Triggering animations.
- Open/Close modal without `prop`. For example, instead of `isOpen` prop you expose `open()` and `close()` methods on a Modal component.

!!! note "Don’t Overuse Refs"
    The stuff that can be done using `props` can be done by `props` and do not use it unneccesary.

ref is reserved keyword - if you pass down ref to child then use another name say `inputRef={textInputRef}` and then access it in child comp using `props.textInputRef`. Then why do we need forwardRef anyway.
