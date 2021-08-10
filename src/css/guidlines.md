# Guidelines

1. keep individual selectors to a single line.
2. Include one space after `:` for each declaration.
3. End all declarations with a semi-colon. The last declaration's is optional, but your code is more error prone without it.
4. Lowercase all hex values, `#fff`.
5. Double Quote attribute values in selectors, `input[type="text"]`.
6. Avoid specifying units for zero values, `margin: 0px;` instead use `margin: 0;`.

```css
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

## Media Query Placement

Place media queries as close to their relevant rule sets whenever possible. Don't bundle them all in a separate style-sheet or at the end of the document.

## Titling

```css
/*------------------------------------*\
  #SECTION-TITLE
\*------------------------------------*/
```

## Naming

Use `-` as separator to name css classes.

```css
.content { }

.sub-content { }
```

> **BEM** style naming conventions can be used for better style.

## Single responsibility principle

The single responsibility principle is a paradigm that, very loosely, states that all pieces of code (in our case, classes) should focus on doing one thing and one thing only.

What this means for us is that our CSS should be composed of a series of much smaller classes that focus on providing very specific and limited functionality. This means that we need to decompose UIs into their smallest component pieces that each serve a single responsibility; they all do just one job, but can be very easily combined and composed to make much more versatile and complex constructs. Let’s take some example CSS that does not adhere to the single responsibility principle:

```css
.error-message {
  display: block;
  padding: 10px;
  border-top: 1px solid #f00;
  border-bottom: 1px solid #f00;
  background-color: #fee;
  color: #f00;
  font-weight: bold;
}

.success-message {
  display: block;
  padding: 10px;
  border-top: 1px solid #0f0;
  border-bottom: 1px solid #0f0;
  background-color: #efe;
  color: #0f0;
  font-weight: bold;
}
```

```html
<div class="error-message">Message</div>
<div class="success-message">Message</div>
```

Here we can see that—despite being named after one very specific use-case—these classes are handling quite a lot: layout, structure, and cosmetics. We also have a lot of repetition. We need to refactor this in order to abstract out some shared objects (OOCSS) and bring it more inline with the single responsibility principle. We can break these two classes out into four much smaller responsibilities:

```css
/**
Order matter here in css. Cascading effect in play
message and message--error have same specifity and as per
cascading message--error override the message properties.
**/
.box {
  display: block;
  padding: 10px;
}


.message {
  border-style: solid;
  border-width: 1px 0;
  font-weight: bold;
  color: grey;
}

.message--error {
  background-color: #fee;
  color: #f00;
}

.message--success {
  background-color: #efe;
  color: #0f0;
}
```

```html
<!-- Order of css classes do not matter here but in css -->
<div class="box message message--error">Message</div>
<div class="box message message--success">Message</div>
```

Now we have a general abstraction for boxes which can live, and be used, completely separately from our message component, and we have a base message component that can be extended by a number of smaller responsibility classes. The amount of repetition has been greatly reduced, and our ability to extend and compose our CSS has been greatly increased. This is a great example of object oriented CSS and the single responsibility principle working in tandem.

## Open/Closed principle

[s]oftware entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

Any additions, new functionality, or features we add to our classes should be added via extension—we should not modify these classes directly. This really trains us to write bulletproof single responsibilities: because we shouldn’t modify objects and abstractions directly, we need to make sure we get them as simple as possible the first time. This means that we should never need to actually change an abstraction—we’d simply stop using it—but any slight variants of it can be made very easily by extending it.

```css
.box {
  display: block;
  padding: 10px;
}

.box--large {
  padding: 20px;
}
```

```html
<div class="box box--large">Box</div>
```

Here we can see that the `.box {}` object is incredibly simple: we’ve stripped it right back into one very small and very focussed responsibility. To modify that box, we extend it with another class; `.box--large {}`. Here the `.box {}` class is closed to modification, but open to being extended.

An incorrect way of achieving the same might look like this:

```css
.box {
  display: block;
  padding: 10px;
}

.content .box {
  padding: 20px;
}
```

Not only is this overly specific, locationally dependent, and potentially displaying poor Selector Intent, we are modifying the `.box {}` directly. A selector like `.content .box {}` is potentially troublesome because it forces all `.box` components to be applicable when placed inside of `.content`.

## DRY

> Do not repeat yourself - [e]very piece of knowledge must have a single, unambiguous, authoritative representation within a system.

**Duplication is better than the wrong abstraction.**

The key isn’t to avoid all repetition, but to normalise and abstract meaningful repetition.

```css
.btn {
  display: inline-block;
  padding: 1em 2em;
  font-weight: bold;
}

.page-title {
  font-size: 3rem;
  line-height: 1.4;
  font-weight: bold;
}

.user-profile__title {
  font-size: 1.2rem;
  line-height: 1.5;
  font-weight: bold;
}
```

From the above code, we can reasonably deduce that the `font-weight: bold`; declaration appears three times purely coincidentally. To try and create an abstraction, mixin, or `@extend` directive to cater for this repetition would be overkill, and would tie these three rulesets together based purely on circumstance.

However, imagine we’re using a web-font that requires `font-weight: bold`; to be declared every time the `font-family` is:

```css
.btn {
  display: inline-block;
  padding: 1em 2em;
  font-family: "My Web Font", sans-serif;
  font-weight: bold;
}

.page-title {
  font-size: 3rem;
  line-height: 1.4;
  font-family: "My Web Font", sans-serif;
  font-weight: bold;
}

.user-profile__title {
  font-size: 1.2rem;
  line-height: 1.5;
  font-family: "My Web Font", sans-serif;
  font-weight: bold;
}
```

Here we’re repeating a more meaningful snippet of CSS; these two declarations have to always be declared together. In this instance, we probably would DRY out our CSS.

```scss
@mixin my-web-font() {
  font-family: "My Web Font", sans-serif;
  font-weight: bold;
}

.btn {
  display: inline-block;
  padding: 1em 2em;
  @include my-web-font();
}

.page-title {
  font-size: 3rem;
  line-height: 1.4;
  @include my-web-font();
}

.user-profile__title {
  font-size: 1.2rem;
  line-height: 1.5;
  @include my-web-font();
}
```

## Composition over inheritance

We should compose more complex composites from a series of much smaller component parts. Nicole Sullivan likens this to using Lego; tiny, single responsibility pieces which can be combined in a number of different quantities and permutations to create a multitude of very different looking results.

This idea of building through composition is not a new one, and is often referred to as composition over inheritance. This principle suggests that larger systems should be composed from much smaller, individual parts, rather than inheriting behaviour from a much larger, monolithic object. This should keep your code decoupled—nothing inherently relies on anything else.

Composition is a very valuable principle for an architecture to make use of, particularly considering the move toward component-based UIs. It will mean you can more easily recycle and reuse functionality, as well rapidly construct larger parts of UI from a known set of composable objects. Think back to our error message example in the Single Responsibility Principle section; we created a complete UI component by composing a number of much smaller and unrelated objects.

```css
/**
Order matter here in css. Cascading effect in play
message and message--error have same specifity and as per
cascading message--error override the message properties.
**/
.box {
  display: block;
  padding: 10px;
}


.message {
  border-style: solid;
  border-width: 1px 0;
  font-weight: bold;
  color: grey;
}

.message--error {
  background-color: #fee;
  color: #f00;
}

.message--success {
  background-color: #efe;
  color: #0f0;
}
```

```html
<!-- Order of css classes do not matter here but in css -->
<div class="box message message--error">Message</div>
<div class="box message message--success">Message</div>
```

## Separation of concerns

The separation of concerns is a principle which, at first, sounds a lot like the single responsibility principle. Modular is a word we’re probably used to; the idea of breaking UIs and CSS into much smaller, composable pieces. The separation of concerns is just a formal definition which covers the concepts of modularity and encapsulation in code. In CSS this means building individual components, and writing code which only focusses itself on one task at a time.

The idea here is to focus fully on one thing at once; build one thing to do its job very well whilst paying as little attention as possible to other facets of your code. Once you have addressed and built all these separate concerns in isolation—meaning they’re probably very modular, decoupled, and encapsulated—you can begin bringing them together into a larger project.

A great example is layout. If you are using a grid system, all of the code pertaining to layout should exist on its own, without including anything else. You’ve written code that handles layout, and that’s it:

```html
<div class="layout">

  <div class="layout__item  two-thirds">
  </div>

  <div class="layout__item  one-third">
  </div>

</div>
```

What lives inside of that layout has separate rulesets.

```html
<div class="layout">

  <div class="layout__item  two-thirds">
    <section class="content">
      ...
    </section>
  </div>

  <div class="layout__item  one-third">
    <section class="sub-content">
      ...
    </section>
  </div>

</div>
```

The separation of concerns allows you to keep code self-sufficient, ignorant, and ultimately a lot more maintainable. Code which adheres to the separation of concerns can be much more confidently modified, edited, extended, and maintained because we know how far its responsibilities reach. We know that modifying layout, for example, will only ever modify layout—nothing else.
