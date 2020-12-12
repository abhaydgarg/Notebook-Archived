# ESLint

- `off` or `0` - turn the rule off.
- `warn` or `1` - turn the rule on as a warning.
- `error` or `2` - turn the rule on as an error.

## File scoped

### Specify the environment

To support Global variables and you no need to add them in `package.json`. For example, test file where we are using `mocha` then ESLInt shows the warning of undefined variables which are actually a global variable of mocha, so by specifying the env we can tell the ESLInt that we are using mocha and do not give warning for mocha's global variables.

```js
/* eslint-env node, mocha */
```

### Global variables

The no-undef rule will warn on variables that are accessed but not defined within the same file. If you are using global variables inside of a file then it’s worthwhile to define those globals so that ESLint will not warn about their usage.

```js
/* global var1, var2 */
```

If you want to optionally specify that these global variables can be written to (rather than only being read), then you can set each with a "writable" flag:

```js
/* global var1:writable, var2:writable */
```

### Specify rule or override rule

`eqeqeq` is turned off and `curly` is turned on as an error.

```js
/* eslint eqeqeq: 'off', curly: 'error' */

// OR

/* eslint eqeqeq: 0, curly: 2 */
```

If a rule has additional options, you can specify them using array literal syntax, such as:

```js
/* eslint quotes: ['error', 'double'], curly: 'error' */
```

### Disable rule

To disable all rule warnings in an entire file.

```js
/* eslint-disable */
```

To disable specific rules for an entire file.

```js
/* eslint-disable no-alert, no-console */
```

## Multiple lines scope

> You need to add "rule enabled" line at the end to enable the rule back, otherwise it disable the rules for all the lines below.

### Disable all rules

```js
/* eslint-disable */

alert('foo');
console.log('bar');

/* eslint-enable */
```

### Disable the specific rules

```js
/* eslint-disable no-alert, no-console */

alert('foo');
console.log('bar');

/* eslint-enable no-alert, no-console */
```

## Line scope

### Same line

#### Disable all rules

```js
alert('foo'); // eslint-disable-line

// OR

alert('foo'); /* eslint-disable-line */
```

#### Disable specific rules

```js
alert('foo'); // eslint-disable-line no-alert, quotes, semi

// OR

alert('foo'); /* eslint-disable-line no-alert, quotes, semi */
```

### Next line

#### Disable all rules

```js
// eslint-disable-next-line
alert('foo');

// OR

/* eslint-disable-next-line */
alert('foo');
```

#### Disable specific rules

```js
// eslint-disable-next-line no-alert, quotes, semi
alert('foo');

// OR

/* eslint-disable-next-line no-alert, quotes, semi */
alert('foo');
```
