# Unit

## What unit to use

When possible, use the `rem` and `em` units to size everything. Not just font size, but also padding, margins, and any length value. This ensures that your design scales up and down uniformly if the user changes their browser's text size. It's common for vision-impaired users to resize their browser up to 200% zoom.

```
# EM #
pixel = font size of element * em
[24 = 16 * 1.5]

# REM #
pixel = font size of <html> element * rem
[24 = 16 * 1.5]
```

## Viewport units (vw & vh)

When you say `50vw` that means what ever the width of viewport, the width of element would be 50% of viewport width. For example, if `vw = 500px` then `50vw` means `250px`. Same is for `vh`.
