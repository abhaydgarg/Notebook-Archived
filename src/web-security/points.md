# Points to note

## Whitelist instead of Blacklist

You should whitelist that means you test or validate which you want.

## Type manipulation

`qs.parse(‘a=foo&b=bar’)` => `{ a: “foo”, b: “bar” }`

`qs.parse(‘a=foo&a=bar’)` => `{ a: [ “foo”, “bar” ] }`

`a` value, which must be string is manipulated into an array by just supplying duplicate `a` in query string.
