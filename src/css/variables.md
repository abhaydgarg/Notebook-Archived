# Variables (CSS custom properties)

Values to be reused throughout a document.

```css
/* Set */
--main-color: black;

/* Access */
color: var(--main-color);
```

## Naming custom properties

```css
/* Wrong: Must be within a selector. */
--foo: 1;

body {
  /* Right. */
  --mainColor: red;
  --main-color: red;

  /* Case-sensitive, therefore both are different. */
  --FOO: 1;
  --Foo: 1;

  /* Wrong: Special characters are not accepted. */
  --color@home: red;
  --black&blue: black;
  --black^2: black;
}
```

## Properties as properties

You can set the value of a custom property with another custom property:

```css
html {
  --red: #a24e34;
  --green: #01f3e6;
  --yellow: #f0e765;

  --error: var(--red);
  --errorBorder: 1px dashed var(--red);
  --ok: var(--green);
  --warning: var(--yellow);
}
```

## Inheritance

```css
body {
  --background: white;
}

.sidebar {
  --background: gray;
}

.module {
  background: var(--background);
}
```

```html
<body> <!-- --background: white -->

  <main>
    <div class="module">
      I will have a white background.
    </div>
  <main>

  <aside class="sidebar"> <!-- --background: gray -->
    <div class="module">
      I will have a gray background.
    </div>
  </aside>

</body>
```

The `module` in the sidebar has a `gray` background because custom properties (like many other CSS properties) inherit through the HTML structure. Each `module` takes the `--background` value from the nearest ancestor.

## Cascading

> Let us to override property.

```css
button {
  --foo: Default;
}

button:hover {
  --foo: I win, when hovered;
  /* This is a more specific selector, so re-setting
     custom properties here will override those in `button` */
}
```

## The :root thing

```css
:root {
  --background: white;
}
```

It’s just a way of setting custom properties as high up as they can go, to make them available globally, or everywhere. **Inheritance plays its role here.**

## Fallback value

If `--color` is not found in inheritance tree then the value is fallback to `red`.

```css
var(--color, red)
```

## @property

The `@property` “at-rule” in CSS allows you to declare the type of a custom property, as well its as initial value and whether it inherits or not.

```css
@property --x {
  syntax: '<number>';
  inherits: false;
  initial-value: 42;
}
```
