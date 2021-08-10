# Efficient selectors

The order of more to less efficient CSS selectors goes:

1. ID `#header`
2. Class `.header`
3. Type `div`
4. Adjacent sibling `h2 + p`
5. Child `li > ul`
6. Descendant `ul a` - **worst**
7. Universal `*`
8. Attribute `[type="text"]`
9. Pseudo-classes/elements `a:hover`

## Reading selectors

You can have standalone selectors such as `#nav`, which will select any element with an ID of 'nav', or you can have combined selectors such as `#nav a`, which will match any anchors within any element with an ID of 'nav'.

Now, we read these left-to-right. We see that we're looking out for `#nav` and then any `a` elements inside there. **Browsers read these differently; browsers read selectors right-to-left.** Where we see a `#nav` with an `a` in it, browsers see an `a` in a `#nav`. This subtle difference has a huge impact on selector performance, and is a very valuable thing to learn.

The right most selector is also known as the **key selector**.

The key selector is what the browser looks for first.

```css
/** .intro is a key selector - the right most **/
#content .intro {}
```

The browser will look for all instances of `.intro` (of which there aren't likely to be many) and then go looking up the DOM tree to see if the matched key selector lives in an element with an ID of `content`.

However, the following selector is not very performant at all:

```css
#content * {}
```

What this does is looks at every single element on the page (that's every single one) and then looks to see if any of those live in the `#content` parent. This is a very un-performant selector as the key selector is a very expensive one.

Using this knowledge we can make better decisions as to our classing and selecting of elements.

### Example

This selector therefore is unreasonably expensive and not very performant:

```css
#social a {}
```

What will happen here is the browser will access all the thousands of links on that page before settling on the four inside of the `#social` section. Our key selector matches far too many other elements that we aren't interested in.

To make selector efficient and fast. Rewrite HTML as follows:

```html
<ul id="social">
 <li><a href="#" class="social-link twitter">Twitter</a></li>
 <li><a href="#" class="social-link facebook">Facebook</a></li>
 <li><a href="#" class="social-link dribble">Dribbble</a></li>
 <li><a href="#" class="social-link gplus">Google+</a></li>
</ul>
```

with more efficient CSS:

```css
#social .social-link {}
```

## Over qualifying selectors

Keep selectors short and limit the number of elements in each selector to **three**. **The single biggest cause of slowdown is too many selectors for CSS rules**

```css
/*Bad*/
html body .wrapper #content a {}

/*Good*/
#content a {}
```

There is just too much going on here, and at least three of these selectors are totally unnecessary. Well the first one means that the browser has to look for all `a` elements, then check that they're in an element with an ID of `content`, then so on and so on right the way up to the html. This is causing the browser way too many checks that we really don't need.

## Avoid the descendant selector

The descendant selector is the most expensive selector in CSS. It is dreadfully expensive, especially if the selector is in the Tag(p, a etc) or Universal Category (\*).

For instance, the performances are so bad, that descendant selectors are banned in Firefox User Interface CSS, without a specific justification. It's a good idea to do the same on your web pages. Try to minimize them as much as possible and even when you use them you should use them judiciously.

```css
/*BAD*/
html body ul li a { }

/*BETTER PERFORMANCE THAN DESCENDANT*/
ul > li > a { }
```

## Avoid qualifying ID and class names with type selectors

```css
/* Not recommended */
ul#example {}
div.error {}

/* Recommended */
#example {}
.error {}
```

## Only be specific when you need to be

Use the most general rule you can to get the job done and only write rules that are more specific when the situation presents itself.

```css
/* The two rules below would accomplish the same thing. */

#primary_content p {
 font-size: 0.875em;
}

p {
 font-size: 0.875em;
}
```

Now it's true that the second rule would apply to every paragraph tag on the site. But at first this is fine. Imagine how simple it is to add more specific rules later. As projects grow and styles become inherently more complex by necessity, the situations where we need to become more specific will naturally present themselves. If we are generic from the start we can take advantage of the cascade and overwrite rules with more specific rules when we need to.

This seems like a fundamental lesson in CSS. We can write styles that apply to many objects and the more specific rules will take precedence.

* The complexity of your style-sheet is directly correlated to how specific your selectors are.

* Re-factoring CSS selectors to be less specific is exponentially more difficult than simply adding specific rules as situations arise.
