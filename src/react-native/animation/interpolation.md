# Interpolation

We set `inputRange` and `outputRange`, so when animation plays as per the value from input range the output value is determined automatically and apply to the property.

Lets say you want to add opacity to be added when you move the card. And based on the value of `x` of card, you set the value for `opacity` property of box.

```js
// set value for drag
this._animatedValue = new Animated.ValueXY();

// set value for opacity
// Note: `this._animatedValue.x` belongs to drag that serves the inputRange and based
// on the value of `x` (x position of card), RN calculate the value for opacity property
// fall within the outputRange
this._opacityAnimation = this._animatedValue.x.interpolate({
    inputRange: [0, 150],
    outputRange: [1, .2]
});

// What the interpolation in this case is doing is as the x value in our _animtedValue
// increases from 0 to 150 it is determine the step and subsequent outputRange between
// 1 and 2.
// So when our x value is 0, our interpolated opacity value will be 1,
// at 150 it will be .2, and when it's at 75 it'll be .4 opacity.
```

## Extrapolate

What to do and how to handle the animation when property based on `inputRange` goes out of range.

In example above: We have set `x` property of card within `[0, 150]`. **What if the value of x goes beyond the 150?**
Well you may think it will automatically stop and our `_opacityAnimation` will stay at `.2`. That isn't the case by default.

You must specify how the interpolation should be extrapolated. If you want it to stop at .2 no matter how far the x goes than you must specify extrapolate:'clamp' like so.

```js
this._opacityAnimation = this._animatedValue.x.interpolate({
    inputRange: [0, 150],
    outputRange: [1, .2],
    extrapolate: 'clamp'
});
```
