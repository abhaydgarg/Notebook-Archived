# Modular CSS

Modular CSS is a collection of principles for writing code that is performant and maintainable at scale.

## Problem

### Difficult to Understand

```html
<div class="box profile pro-user">
  <img class="avatar image" />
  <p class="bio">...</p>
</div>
```

How are the classes `box` and `profile` related to each other? How are the classes `profile` and `avatar` related to each other? Are they related at all? Should you be using `pro-user` alongside `bio`? Will the classes `image` and `profile` live in the same part of the CSS? Can you use `avatar` anywhere else?

There’s no way to answer those questions from here. You have to do a bunch of detective work in the CSS.

### Difficult to Reuse

Reusing code can be surprisingly tricky. Let’s say there’s a style on one page that you want to reuse on another page, but when you try, you find out it was written in a way that only works on the first page. The author assumed it was living inside a particular element or that it was inheriting certain classes from the page. It doesn’t work at all in a different context. You don’t want to break the original, so you duplicate the code.

Now you’ve got two problems: You’ve got your original code and your duplicated code. You’ve doubled your maintenance burden.

### Difficult to Maintain

CSS at scale can also be challenging to maintain. You change the markup, and the styles collapse like a house of cards. You want to update a style on one page, and it breaks on another. You try to override the other page, but get caught in a specificity war.

## Solution - Modularity

> The separation of concerns allows you to keep code self-sufficient, ignorant, and ultimately a lot more maintainable. Code which adheres to the separation of concerns can be much more confidently modified, edited, extended, and maintained because we know how far its responsibilities reach. We know that modifying layout, for example, will only ever modify layout—nothing else.

Developer think of the page as a complete unit, and of the smaller pieces as belonging to the page. That approach is top-down thinking and results in code that’s full of one-offs and special bits that only live a single page. It’s not conducive to writing reusable code.

What Modular CSS asks you to do is step back, and instead of thinking about this at the page level, look at the fact that your page is made up of small chunks of discrete content. This isn’t a page. This is a collection of pieces.

You’ve got a logo, a search bar, navigation, a photo list, a secondary nav, a tabbed box, a video player, etc. These are discrete pieces of content that could be used anywhere in your site. They just happen to be assembled this way on this particular page.

Modular CSS is bottom-up thinking. It asks you to start with the reusable building blocks that your entire site is constructed from.

Does that remind you of anything? It should! The Lego analogy is used by almost everyone who writes about Modular CSS for a good reason. The idea of building a UI out of standardized, easy-to-understand blocks that behave predictably regardless of context is a great concept.

### Rules

#### Separation of container from content

> Context-Independent

That means that objects should look the same no matter where you put them. In other words, avoid location-dependent styles. Objects should not be styled based on their context.

For example, rather than make all buttons in the sidebar orange, and all buttons in the main area blue, you should make a button class that’s blue, and a modifier that’s orange. Then your orange buttons can be used anywhere because they’re not tied to the sidebar, they’re just one of your button styles.

As a general rule, **styles should never be scoped to particular containers.** Otherwise, you’ll be unable to reuse them without applying overrides. For example, below is the standard way of setting up the elements that make up a sidebar:

```css
#sidebar {
    padding: 2px;
    left: 0;
    margin: 3px;
    position: absolute;
    width: 140px;
}

#sidebar .list {
    margin: 3px;
}

#sidebar .list .list-header {
    font-size: 16px;
    color: red;
}

#sidebar .list .list-body {
    font-size: 12px;
    color: #FFF;
    background-color: red;
}
```

Now, here are the same coding instructions with the content and containers separated. Avoiding child selectors is a good strategy for maintaining separation between content and containers.

```css
.sidebar {
    padding: 2px;
    left: 0;
    margin: 3px;
    position: absolute;
    width: 140px;
}

.list {
    margin: 3px;
}

.list-header {
    font-size: 16px;
    color: red
}

.list-body {
    font-size: 12px;
    color: #FFF;
    background-color: red;
}
```

#### Separation of structure from skin

That means to define the repeating patterns as reusable structures. For example, what makes a button look like a button? You might say, “rounded corners, padding, and a border.” Then you may have repeating visual patterns that serve as repeating skins. For example, you may have some blue buttons and red buttons. Those are two separate skins, as they each have a different background color.

The structure of an application refers to  **things that are “invisible”**  to the user such as instructions for element size and positioning. These properties include:

- Height
- Width
- Margins
- Padding
- Overflow

An application’s skin refers to the  **visual properties**  of elements such as:

- Colors
- Fonts
- Shadows
- Gradients

In other words, the structure consists of the instructions for  **how things are laid out**, and the skin defines  **what the layout looks like**.

```css
/** BEFORE - Structure + Skin **/
#button {
  width: 200px;
  height: 50px;
  padding: 10px;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

/** AFTER **/

/* Structure */
.button {
  width: 200px;
  height: 50px;
}

/* Skin variants */
.button-default {
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

.button-dark {
  border: solid 1px #ccc;
  background: linear-gradient(#000, #333);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}
```

#### No nesting hell

> Don’t nest deeper than one level.

Use classes to name your objects and their child elements so markup can change without impacting style.

You don’t want your markup to determine your CSS. So if you change the headline from an `h1` to an `h4`, you shouldn’t have to update your CSS. That headline should have a class on it, and that class will be applied regardless of what element you choose. For example, your navigation should be something like `.site-nav`  _not_  `#header ul`.

Example of nesting hell:

```html
<body>
  <div class="container">
    <div class="content">
      <div class="articles">
        <div class="post">
          <div class="title">
            <h1><a href="#">Hello World</a>
          </div>
          <div class="content">
            <p></p>
            <ul>
              <li>...</li>
            </ul>
          </div>
          <div class="author">
            <a href="#" class="display"><img src="..." /></a>
            <h4><a href="#">...</a></h4>
            <p>
              <a href="#">...</a>
              <ul>
                <li>...</li>
              </ul>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</body>
```

Because Sass offers you the ability to nest your selectors, and you know that having your code encapsulated is the "good way to avoid collisions with other styles", you might find yourself mimicking the DOM in your Sass

```scss
body {
  div.container {
    div.content {
      div.articles {
        & > div.post {
          div.title {
            h1 {
              a {
              }
            }
          }
          div.content {
            p { ... }
            ul {
              li { ... }
            }
          }
          div.author {
            a.display {
              img { ... }
            }
            h4 {
              a { ... }
            }
            p {
              a { ... }
            }
            ul {
              li { ... }
            }
          }
        }
      }
    }
  }
}

// Compiles to - CSS monster
body { ... }
body div.content div.container { ... }
body div.content div.container div.articles { ... }
body div.content div.container div.articles > div.post { ... }
body div.content div.container div.articles > div.post div.title { ... }
body div.content div.container div.articles > div.post div.title h1 { ... }
body div.content div.container div.articles > div.post div.title h1 a { ... }
body div.content div.container div.articles > div.post div.content { ... }
body div.content div.container div.articles > div.post div.content p { ... }
body div.content div.container div.articles > div.post div.content ul { ... }
body div.content div.container div.articles > div.post div.content ul li { ... }
body div.content div.container div.articles > div.post div.author { ... }
body div.content div.container div.articles > div.post div.author a.display { ... }
body div.content div.container div.articles > div.post div.author a.display img { ... }
body div.content div.container div.articles > div.post div.author h4 { ... }
body div.content div.container div.articles > div.post div.author h4 a { ... }
body div.content div.container div.articles > div.post div.author p { ... }
body div.content div.container div.articles > div.post div.author p a { ... }
body div.content div.container div.articles > div.post div.author ul { ... }
body div.content div.container div.articles > div.post div.author ul li { ... }
```

There are so many reasons why this is just plain wrong, from rendering performance to file-size performance. Just think about how many bytes this will add to your CSS. But that's not the only problem! Since your styles are so specific to the DOM, maintainability is now a problem.

Any change you make to your markup will need to be reflected into your Sass and vice versa. It also means that the styles are bounded for life to the those elements and that HTML structure which completely defeats the purpose of the "Cascade" part of "Cascading Style Sheets."

##### Solution

> Take separate markup's example.

```html
<div class="media">

  <a href="#" class="media__image">
    <img src="/profile_image.jpg" class="media__avatar" />
  </a>

  <div class="media__description">
    @Stubbornella
  </div>

</div>
```

```css
/** with the Sass ampersand character **/
.media{
  margin:10px;
  &__description{
    overflow: hidden;
  }
  &__image{
    float: left;
    margin-right: 10px;
  }
  &__avatar{
    display: block;
  }
}
```

Notice how everything have a unique class of its own and sub-classes are only nested by the BEM block-element delimiter. This way we save a code from unwanted specificity spikes and keep it easier to maintain.

> Having unique class by following BEM naming convention, provides a namespace, so we don’t need to nest anymore. Even though everything is a single class at the root level of your CSS, the names are specific enough to avoid conflicts.

#### Don’t Use IDs

IDs mess up specificity because they’re too strong, but more importantly, objects are meant to be reusable. IDs are, by definition, unique. So if you put an ID on your object, you can’t reuse it on the same page and are missing the point of a modular object.

## Conclusion

Let’s have a brief recap. Remember this?

```html
<div class="box profile pro-user">
  <img class="avatar image" />
  <p class="bio">...</p>
</div>
```

How are the classes `box` and `profile` related to each other? How are the classes `profile` and `avatar` related to each other? Are they related at all? Should you be using `pro-user` alongside `bio`? Will the classes `image` and `profile` live in the same part of the CSS? Can you use `avatar` anywhere else?

Now we know how to address all those concerns. By writing Modular CSS and using a proper naming convention, we can produce self-documenting code:

```html
<div class="box profile profile--is-pro-user">
  <img class="avatar profile__image" />
  <p class="profile__bio">...</p>
</div>
```

We can see which classes are and are not related to each other, and how. We know what classes we can’t use outside of the scope of this component. Also, we know which classes we are free to reuse elsewhere.

### A few guidlines

1. Objects should have a single responsibility; they should do one thing well and one thing only.
They should be completely encapsulated; the object should be able to stand on its own and work the same everywhere.
2. Use classes for the object, modifier, and state. This allows you the flexibility to mix and match these without running the risk of being too specific.
3. Pick a naming system and be consistent. There are different naming formats, so form a plan as a team. For example, camelCase or no camelcase? And how will you identify children, states, modifiers…the BEM way `(.block__element--modifier)`?
4. Utilize a CSS preprocessor, such as Sass. Use partials, nesting, and mixins. These help to dry up the repetition.
