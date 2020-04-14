# Type of elements

Each HTML element rendered on the screen come in two flavors: **block** elements and **inline** elements.

## Block elements

* Block elements always appear or start on a new line and takes up the full width available of parent (stretches out to the left and right as far as it can).

* The default height of block elements is based on the content it contains.

* Generally, block-level elements may contain inline elements and other block-level elements.

For instance, `<h1>` and `<p>`, `<div>`, `<img>` are block-level elements.

## Inline elements

* Inline elements don’t affect vertical spacing that means an inline element does not start on a new line and only takes up as much width as necessary.
* Inline elements typically may only contain text and other inline elements.
* `width`, `height` and vertical `margin` values are ignored in inline element. You cannot provide width and height to it.

For instance `<em>` and `<strong>` are inline elements.

## What is Normal Flow

Normal flow positions elements as they appear in the HTML. During normal flow, block elements and inline elements are rendered differently.

> `position: absolute|relative|fixed` and `display: float` properties that take the element out of the normal flow and used mostly for positioning element the way we want ignoring the normal flow.

**Block elements** are positioned on a page one after the other (in the order they're written in the HTML). They start in the upper left of the containing or parent box and stack from top to bottom **(vertically)** in new line for each block element.

> Multiple block elements always stack top to bottom in new line even if the width of block element is smaller to its parent. That means the leftover space on the right will be remain un occupied, if the next element to be laid out again is a block element. So in normal flow, building a layout like laying out `<div>` side by side is not possible unless we use the property `float` or in other word we move block element out from normal flow using `float`. **That is why, floating element was the mostly used technique to build page layout** until Flexbox.

**Inline elements** are laid out **horizontally**, one after the other beginning at the top of the container or parent element. When there isn't enough space to fit all the elements of the inline box on one line, they wrap to the next line.
