# BEM

BEM – meaning **block**, **element**, **modifier** – is a front-end naming methodology thought up by the guys at Yandex. It is a smart way of naming your CSS classes to give them more transparency and meaning to other developers. They are far more strict and informative, which makes the BEM naming convention ideal for teams of developers on larger projects that might last a while.

```css
.block {}
.block__element {}
.block--modifier {}
```

- `.block` represents the higher level of an abstraction or component.
- `.block__element` represents a descendent of `.block` that helps form `.block` as a whole.
- `.block--modifier` represents a different state or version of `.block`.

```html
<form class="site-search  site-search--full">
  <input type="text" class="site-search__field">
  <input type="Submit" value ="Search" class="site-search__button">
</form>
```

We can see that we have a block called `.site-search` which has an element which lives inside it called `.site-search__field`. We can also see that there is a variation of the `.site-search` called `.site-search--full`.

## Rule

> Only single **class** selector and no nested classes which creates descendent selectors, child selectors and combined selectors etc because nested classes increase the specifity and later hard to override because modifier class has to match the specifity to use cascading effect. For example, just use `.btn__price` not `.btn .btn__price`.

Just use single class and if want to modify the rule then just create modifier class and use it. Make sure the modifier class is placed after the class you want to modify its style's properties to make CSS cascading rule take in effect.

## Why class only

Efficient selector and can be used easily for modular approach. It is the only selector that allows you to isolate the styles of each component in the project; increase the readability of the code and do not limit the re-use of the layout.

To answer it, let's answer why not other selectros.

### Why don’t we use IDs?

The ID provides a unique name for an HTML element. If the name is unique, you can’t reuse it in the interface. This prevents you from reusing the code.

### Why don’t we use tag selectors?

HTML page markup is unstable: A new design can change the nesting of the sections, heading levels (for example, from `<h1>` to `<h3>`) or turn the `<p>` paragraph into the `<div>`tag. Any of these changes will break styles that are written for tags.

### Why don’t we use nested selectors?

Nested selectors increase code coupling and make it difficult to reuse the code. The BEM methodology doesn’t prohibit nested selectors, but it recommends not to use them too much. For example, nesting is appropriate if you need to change styles of the elements depending on the block’s state or its assigned theme.

```css
.button_hovered .button__text {
  text-decoration: underline;
}

.button_theme_islands .button__text {
  line-height: 1.5;
}
```

### Why don’t we combine a tag and a class in a selector?

Combining a tag and a class in the same selector (for example, `button.button`) makes CSS rules more specific, so it is more difficult to redefine them.

```html
<button class="button">...</button>
```

Let’s say you set CSS rules in the `button.button` selector. Then you add the `active` modifier to the block:

```html
<button class="button button_active">...</button>
```

The `.button_active` selector doesn’t redefine the block properties written as `button.button` because `button.button` is more specific than `.button_active`. To make it more specific, you should combine the block modifier selector with the `button.button_active` tag.

### Why don’t we use combined selectors?

Combined selectors are more specific than single selectors, which makes it more difficult to redefine blocks.

```html
<button class="button button_theme_islands">...</button>
```

Let’s say you set CSS rules in the `.button.button_theme_islands` selector to do less writing. Then you add the “active” modifier to the block:

```html
<button class="button button_theme_islands button_active">...</button>
```

The `.button_active` selector doesn’t redefine the block properties written as `.button.button_theme_islands`because `.button.button_theme_islands` is more specific than `.button_active`. To redefine it, combine the block modifier selector with the `.button` selector and declare it below the `.button.button_theme_islands` because both selectors are equally specific:

```css
.button.button_theme_islands {}
.button.button_active {}
```

If you use simple class selectors, you won’t have problems redefining the styles:

```css
.button_theme_islands {}
.button_active {}
.button {}
```

## Common problems

### What To Do About ‘Grandchild’ Selectors (And Beyond)?

```html
<div class="card">

    <div class="card__header">
        <!-- Here comes the grandchild… -->
        <h2 class="card__header__title">Title text here</h2>
    </div>

    <div class="card__body">

        <img class="card__body__img" src="some-img.png" alt="description">
        <p class="card__body__text">Lorem ipsum dolor sit amet, consectetur</p>
        <p class="card__body__text">Adipiscing elit.
            <a href="/somelink.html" class="card__body__text__link">Pellentesque amet</a>
        </p>

    </div>
</div>
```

Double-underscore pattern should appear only once in a selector name. BEM stands for `Block__Element–Modifier`, **not**  `Block__Element__Element–Modifier`. So, avoid multiple element level naming. If you’re getting to grandchild levels, then you’ll probably want to revisit your component structure anyway.

BEM naming isn’t strictly tied to the DOM, so it doesn’t matter how many levels deep a descendent element is nested. The naming convention is there to help you identify relationships with the top-level component block — in this case, `card`.

```html
<div class="card">
    <div class="card__header">
        <h2 class="card__title">Title text here</h2>
    </div>

    <div class="card__body">

        <img class="card__img" src="some-img.png" alt="description">
        <p class="card__text">Lorem ipsum dolor sit amet, consectetur</p>
        <p class="card__text">Adipiscing elit.
            <a href="/somelink.html" class="card__link">Pellentesque amet</a>
        </p>

    </div>
</div>
```

This means that all of the descendent elements will be affected only by the `card` block. So, we would be able to move the text and images into `card__header`.

### Cross-Component… Components?

Another issue commonly faced is a component whose styling or positioning is affected by its parent container.

To summarize, let’s assume we want to add a `button` into the `card__body`. The `button` is already in its own component and is marked up like this:

```html
<div class="card">
    <div class="card__header">
        <h2 class="card__title">Title text here</h3>
    </div>

    <div class="card__body">

        <img class="card__img" src="some-img.png">
        <p class="card__text">Lorem ipsum dolor sit amet, consectetur</p>
        <p class="card__text">Adipiscing elit. Pellentesque.</p>

        <!-- Our nested button component -->
        <button class="button button--primary">Click me!</button>

    </div>
</div>
```

Now, we want to modify the button, for example, we want to make it a bit smaller, with fully rounded corners, but only when it’s a part of a `card` component.

Your best bet is to use a modifier for these small cosmetic differences, because you may well find that you wish to reuse them elsewhere as your project grows.

```html
<button class="button button--rounded button--small">Click me!</button>
```

### How To Handle States?

This is a common problem, particularly when you’re styling a component in an active or open state. Let’s say our cards have an active state; so, when clicked on, they stand out with a nice border styling treatment. How do you go about naming that class?

```html
<!-- standalone state hook -->
<div class="card is-active">
    […]
</div>

<!-- or BEM modifier -->
<div class="card card--is-active">
    […]
</div>
```

### How To Nest Components?

Suppose we want to display a checklist in our `card` component. Here is a demonstation of how not to mark this up:

```html
<div class="card">
    <div class="card__header">
        <h2 class="card__title">Title text here</h3>
    </div>

    <div class="card__body">

        <p>I would like to buy:</p>

        <!-- Uh oh! A nested component -->
        <ul class="card__checklist">
            <li class="card__checklist__item">
                <input id="option_1" type="checkbox" name="checkbox" class="card__checklist__input">
                <label for="option_1" class="card__checklist__label">Apples</label>
            </li>
            <li class="card__checklist__item">
                <input id="option_2" type="checkbox" name="checkbox" class="card__checklist__input">
                <label for="option_2" class="card__checklist__label">Pears</label>
            </li>
        </ul>

    </div>
    <!-- .card__body -->
</div>
<!-- .card -->
```

We have a couple of problems here. One is the grandparent selector. The second is that all of the styles applied to `card__checklist__item` are scoped to this specific use case and won’t be reusable.

Preference here would be to break out the checklist items into their own components, enabling them to be used independently elsewhere.

```html
<div class="card">
    <div class="card__header">
        <h2 class="card__title">Title text here</h3>
    </div>

    <div class="card__body"><div class="card__body">

        <p>I would like to buy:</p>

        <!-- Much nicer - a wrapper module -->
        <ul class="list">
            <li class="list__item">

                <!-- A reusable nested component -->
                <div class="checkbox">
                    <input id="option_1" type="checkbox" name="checkbox" class="checkbox__input">
                    <label for="option_1" class="checkbox__label">Apples</label>
                </div>

            </li>
            <li class="list__item">

                <div class="checkbox">
                    <input id="option_2" type="checkbox" name="checkbox" class="checkbox__input">
                    <label for="option_2" class="checkbox__label">Pears</label>
                </div>

            </li>
        </ul>
        <!-- .list -->
    </div>
    <!-- .card__body -->
</div>
<!-- .card -->
```

This saves you from having to repeat the styles, and it means we can use both the `list` and `checkbox` in other areas of our application. It does mean a little more markup, but it’s a small price to pay for readability, encapsulation and reusability. You’ve probably noticed these are common themes!

### Won’t Components End Up With A Million Classes?

Some argue that having a lot of classes per element is not great, and `–-modifiers` can certainly add up. It means the code is more readable and I know exactly what it is supposed to be doing.

For context, this is an example of four classes being needed to style a button:

```html
<button class="c-button c-button--primary c-button--huge  is-active">Click me!</button>
```

## With SCSS

### The power of the ampersand (&)

We want scss compile to:

```css
.MyComponent {}
.MyComponent-title {}
.MyComponent-content {}
```

```scss
.MyComponent {
  .MyComponent-title {}
  .MyComponent-content {}
}

// Compiles to
// Not what we want. Unnecessary specificity!
// and desecndent selectors.
.MyComponent .MyComponent-title {}
.MyComponent .MyComponent-content {}
```

Use `&`.

```scss
.MyComponent {
  &-title {}

  &-content {}
}

// Compiles to
.MyComponent {}
.MyComponent-title {}
.MyComponent-content {}
```

There are times when the component needs a modifier.

```html
<div class="MyComponent MyComponent--xmasTheme"></div>
```

```scss
.MyComponent {
  &--xmasTheme {}
}

// Compiles to
.MyComponent {}
.MyComponent--xmasTheme {}
```

But, what about modifying the child elements?

```scss
.MyComponent {
  &-title {
    .MyComponent--xmasTheme & {
    }
  }

  &-content {
    .MyComponent--xmasTheme & {
    }
  }
}

// Compiles to
.MyComponent-title {}
.MyComponent--xmasTheme .MyComponent-title {}
.MyComponent-content {}
.MyComponent--xmasTheme .MyComponent-content {}
```

This gets the job done, but there is a better way by using variable.

```scss
// Sass string interpolation syntax is #{VARIABLE}
$block: ".MyComponent";

#{$block} {
  &-title {
    #{$block}--xmasTheme & {
    }
  }
}

// Compiles to
.MyComponent {}
.MyComponent-title {}
.MyComponent--xmasTheme .MyComponent-title {}
```

That’s cool, but the variable is globally scoped. We can fix that by creating the `$block` variable inside the component declaration, which would scope it to that component. Then we can re-use the `$block` variable in other components.

```scss
.MyComponent {
  $block: '.MyComponent';

  &-title {
    #{$block}--xmasTheme & {
    }
  }

  &-content {
    #{$block}--xmasTheme & {
    }
  }
}

// Compiles to
.MyComponent {}
.MyComponent-title {}
.MyComponent--xmasTheme .MyComponent-title {}
.MyComponent-content {}
.MyComponent--xmasTheme .MyComponent-content {}
```

This is closer, but again, we have to write the theme name over and over. However, we can improve this even further. Variables can also store the value of the ampersand!

```scss
.MyComponent {
  $block: &;
  $xmasTheme: #{&}--xmasTheme;

  &-title {
    #{$xmasTheme} & {
    }
  }
}

// Still compiles to
.MyComponent {}
.MyComponent-title {}
.MyComponent--xmasTheme .MyComponent-title {}
```
