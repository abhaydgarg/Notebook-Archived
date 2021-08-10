# Animated

Unlike`LayoutAnimation`, the`Animated`library is extremely configurable and can target very specific components. However, it currently runs on the`JavaScript`side, utilizes`requestAnimationFrame`to animate the values and`setNativeProps`in order to send specific values back to the native world.

This can have some down falls. Most notably, because it's running in the JavaScript world, in the event you have a`setState`that triggers a diff, and thus may take some time, you can see your animations stutter.

> By default, Animated gives us access to 3 animatable components: `Animated.View`, `Animated.Text` and `Animated.Image`.

## Animated.Value

> For single value

To animate value you first create it using `Animated.Value`. It is required in order to attach listener to the value that is being animated for component like `Animated.View`.

You set value either in `constructor` or in `componentWillMount`.

### setValue

`setValue` is a function exposed on the `new Animated.Value()` instance. It allows for external code to control the internal value of the instance without triggering animations of intermediary states.

```js
this._animatedValue = new Animated.Value(0);

<Animated.View style={{left: this._animatedValue}} />

this._animatedValue.setValue(150);

// The Animated.View will immediately jump from position left 0 to positoin left 150
// and will not iterate nicely from 0 to 150. You must use other Animated functions to do so.
```

### addListener/removeListener

Animation is asynchronous, but in some cases you may want to keep track of what the animated values are.

```js
this._animatedValue = new Animated.Value(0);

var animatedListenerId = this._animatedValue.addListener(({value}) => this._value = value);

Animated.timing(this._animatedValue, {
    toValue: 100,
    duration: 500
}).start();

componentWillUnmount() {
    // remove listener
    this._animatedValue.removeListener(animatedListenerId);
    // OR remove all listeners
    this._animatedValue.removeAllListeners()
}
```

### stopAnimation

You can stop animation in progress using `stopAnimation`.

```js
this._animatedValue = new Animated.Value(0);

Animated.timing(this._animatedValue, {
    toValue: 100,
    duration: 500
}).start();


setTimeout(() => {
  this._animatedValue.stopAnimation(({value}) => console.log("Final Value: " + value))
}, 250);

// after 250 milliseconds, we'll stop the 500 millisecond animation.
// We also log the final value to the console, which will be roughly somewhere around 50.
```

## Animated.ValueXY

> For vectors.

It is often used to help with position of components and or when you're dealing with gestures.

It has all the methods that are available for `Animated.Value` and a few additional helper methods. `x` and `y` values are instance of `Animated.Value`.

```js
constructor(props) {
    super(props)
    this.state = {
        someAnimatedValue: new Animate.ValueXY()
    }
}
// They default to {x: 0, y:0}. However, if you want to change the default values,
// you can pass in an object, such as new Animated.ValueXY({x: 15, y: 15}),
// which would set the default internal Animated.Values both to 15.
```

### getLayout

```js
this._animatedValue = new Animated.ValueXY();

// behind scene gives us following
{
    left: this._animatedValue.x,
    top: this._animatedValue.y
}

// That would be provided to an Animated.View or other Animated component.
<Animated.View style={{
    left: this._animatedValue.x,
    top: this._animatedValue.y
}} />

// But with the helper method you would only need to do
<Animated.Value style={this._animatedValue.getLayout()} />
```

### getTranslateTransform

```js
this._animatedValue = new Animated.ValueXY();

// The equivalence of what it generates is something like so:
// transform: [
//     {
//         translateX: this._animatedValue.x
//     },
//     {
//         translateY: this._animatedValue.y
//     }
// ]
// That would be provided to an Animated.View or other Animated component.
<Animated.View style={{
    transform: [{translateX: this._animatedValue.x}, {translateY: this._animatedValue.y}]
  }}
/>

// But with the helper method you would only need to do
<Animated.Value style={{transform: this._animatedValue.getTranslateTransform()}} />
```

## start

```js
// trigger animation and call a callback

this._animatedValue = new Animated.Value(0);

Animated.timing(this._animatedValue, {
    toValue: 100,
    duration: 500
}).start(() => console.log('done'));
```

## Animated.timing

Runs an animation for defined time. It tells how long the one cycle of animation should run.

```js
// This animation will wait 300 milliseconds before triggering and
// then take 500 milliseconds to animate from 0 to 100.
// The total time of animation will be 800 milliseconds.
this._animatedValue = new Animated.Value(0);

Animated.timing(this._animatedValue, {
    toValue: 100,
    delay: 300,
    duration: 500
}).start()
```

You can also mention speed of the animation with `easing` property.

**Default** is `easeInOut` - slow start and slow end.

```js
this._animatedValue = new Animated.Value(0);

Animated.timing(this._animatedValue, {
    toValue: 100,
    easing: Easing.linear,
    duration: 500
}).start()

Animated.timing(this._animatedValue, {
    toValue: 100,
    easing: Easing.elastic(2), // Springy
    duration: 500
}).start()

Animated.timing(this._animatedValue, {
    toValue: 100,
    easing: Easing.bounce, // Like a ball
    duration: 500
}).start()
```

## Animated.spring

It gives you spring like animation like bounce and rebound effect.

```js
constructor () {
  super()
  this.springValue = new Animated.Value(0.3)
}

Animated.spring(
    this.springValue,
    {
      toValue: 1,
      friction: 1
    }
  ).start()

// For Animated.ValueXY, you can supply an object {x: number y: number} in the toValue.
```

## Animated.decay

The typical use of decay is to take something that may be moving at a particular velocity and apply deceleration to it.

It starts with an initial velocity and gradually slows to a stop.

## Animated.sequence

Starts an array of animations in order, waiting for each to complete before starting the next. If the current running animation is stopped, no following animations will be started.

```js
this._opacityAnimationValue = new Animated.Value(1);
this._moveAnimationValue = new Animated.ValueXY();

Animated.sequence([
    Animated.timing(this._moveAnimationValue, {
        toValue: 100,
        duration: 500
    }),
    Animated.timing(this._opacityAnimationValue, {
        toValue: 0,
        duration: 200
    })
]).start()

<Animated.View style={{opacity: this._opacityAnimationValue, transform: this._moveAnimationValue.getTranslateTransform()}} />

// We have 2 animations. One that will control opacity, and the
// other movement ( or in our case translateX/translateY).
// The first animation will move the View from x: 0, y: 0 to x: 100, y: 100.
// Then our second animation will kick off and it will fade out the opacity from 1 to 0.
```

## Animated.parallel

The parallel call will have all defined animations in the array trigger at the same time.

```js
this._opacityAnimationValue = new Animated.Value(1);
this._moveAnimationValue = new Animated.ValueXY();

Animated.parallel([
    Animated.timing(this._moveAnimationValue, {
        toValue: 100,
        duration: 500
    }),
    Animated.timing(this._opacityAnimationValue, {
        toValue: 0,
        duration: 200
    })
]).start()

<Animated.View style={{opacity: this._opacityAnimationValue, transform: this._moveAnimationValue.getTranslateTransform()}} />

// We have 2 animations. One that will control opacity, and the other
// movement ( or in our case translateX/translateY).
// The first animation will move the View from x: 0, y: 0 to x: 100, y: 100.
// At the same time our second animation will kick off and it will fade out the
// opacity from 1 to 0. So the Animated.View will move and fade out at the same time.
```

## Animated.stagger

It is just like a `Animated.parallel`, but allows you to add delays to the animations.

```js
this._opacityAnimationValue = new Animated.Value(1);
this._moveAnimationValue = new Animated.ValueXY();

Animated.stagger(100, [
    Animated.timing(this._moveAnimationValue, {
        toValue: 100,
        duration: 500
    }),
    Animated.timing(this._opacityAnimationValue, {
        toValue: 0,
        duration: 200
    })
]).start()

<Animated.View style={{opacity: this._opacityAnimationValue, transform: this._moveAnimationValue.getTranslateTransform()}} />

// We have 2 animations. One that will control opacity, and the other
// movement ( or in our case translateX/translateY ).
// The first animation will move the View from x: 0, y: 0 to x: 100, y: 100.
// This will happen over the 500 milliseconds.
// There will be a delay of 100 milliseconds.
// Then our second animation will kick off and it will fade out the opacity from 1 to 0.
// So the Animated.View will move, wait 100 milliseconds and then fade out.
```

## Animated.delay

The essense of `Animated.delay` internals is that it creates an `Animated.timing` that does nothing but wait. The main purpose is for using with composing animations in stagger, or sequence.

```js

// Both are equivalent, start animation after 300 ms.

// ### 1

this._animatedValue = new Animated.Value(0);

Animated.timing(this._animatedValue, {
    toValue: 100,
    delay: 300,
    duration: 500
}).start()

// ### 2

this._animatedValue = new Animated.Value(0);

Animated.sequence([
    Animated.delay(300),
    Animated.timing(this._animatedValue, {
        toValue: 100,
        duration: 500
    })
]).start()

```

## createAnimatedComponent

Animated ships with 3 default views. Animated.View, Animated.Text, and Animated.Image. However in some cases you may want to turn a component into an Animated component so it can take advantage of Animated.Value in it's styles or properties.

```js
  // making ScrollView an animated component
  var AnimatedScrollView = Animated.createAnimatedComponent(ScrollView)
```
