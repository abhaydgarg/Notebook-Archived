# JSON

[Guidelines](https://google.github.io/styleguide/jsoncstyleguide.xml)

## null and empty value

It is good programming practice to return an empty `array` if the expected return type is an `array` and empty `object` if the expected return type is `object`. This makes sure that the receiver of the json can treat the value as an `array` and `object` immediately and does not first has to check if it is `null`. `String`, `Boolean` and `Number` do not have an **empty** form, so there it is okay to use `null` value.

!!! note
    `String`, empty quotes `''` does not represent empty value. `''` still represents a value. Same for `Number`, 0 does not represent empty value.

    `null` value just says that property has no value.

### Should null value really be used or should be remove the property

If a property is optional or has an empty or `null` value, consider dropping the property from the JSON, unless there's a strong semantic reason for its existence.

> If really want to use `null` for optional and missing value then do not use it for array and object. For these two use `[], {}` for empty value and for other types use `null`.

## Singular vs plural property names

Array types should have plural property names. All other property names should be singular.

Arrays usually contain multiple items, and a plural property name reflects this. The  `items`  property name is plural because it represents an array of item objects. Most of the other fields are singular.

There may be exceptions to this, especially when referring to numeric property values. For example, `totalItems`  makes more sense than  `totalItem`. The field name could also be changed to  `itemCount`  to look singular.

```js
{
  // Singular
  "author": "lisa",
  // An array of siblings, plural
  "siblings": [ "bart", "maggie"],
  // "totalItem" doesn't sound right
  "totalItems": 10,
  // But maybe "itemCount" is better
  "itemCount": 10,
}
```

## Use double quotes

If a property requires quotes, double quotes must be used. All property names must be surrounded by double quotes. Property values of type string must be surrounded by double quotes. Other value types (like boolean or number) should not be surrounded by double quotes.

## Choose meaningful property names

- Property names should be meaningful names with defined semantics.
- Property names must be camel-cased, ascii strings.
- The first character must be a letter, an underscore (_) or a dollar sign ($).
- Subsequent characters can be a letter, a digit, an underscore, or a dollar sign.
- Reserved JavaScript keywords should be avoided

## Dates should be formatted as recommended by RFC 3339

```js
{
  "lastUpdate": "2007-11-06T16:34:41.000Z"
}
```
