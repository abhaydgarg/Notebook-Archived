# NPM

## semver (semantic versioning)

It is intuitive way to know what have been introduced in new version by comparing with old version number.

> Major.Minor.Patch

- Major: Breaks the API and not backward compatible.
- Minor: Add new features but does not break the API.
- Patches: Bug fixes.

### Pre-release tags

- Major.Minor.Patch-alpha.number: `1.2.3-alpha.3`.
- Major.Minor.Patch-beta.number: `1.2.3-beta.3`

## Version ranges

### Simple ranges

- `<` Less than
- `<=` Less than or equal to
- `>` Greater than
- `>=` Greater than or equal to
- `=` Equal. If no operator is specified, then equality is assumed.

`>=1.2.7 would match the versions 1.2.7, 1.2.8, 2.5.3, and 1.3.9, but not the versions 1.2.6 or 1.1.0.`

Use of `||`.

`1.4.0 || >= 2.4.0`: Matches the version `1.4.0`, but also every version equal or greater than `2.4.0`. Doesn’t match `1.3.5` or `2.3.9`.

### Hyphen ranges

`1.4.0–1.5.2`: Match every version from `1.4.0` to `1.5.2`.

### X ranges

Any of X, x, or * may be used to “stand in” for one of the numeric values in the [major, minor, patch] tuple.

- `*` || `x`: Any version.
- `1.4.x`: Match `1.4.0`, `1.4.1`, `1.4.2`, and so on.
- `1.x.x`: Match every version that begins with a `1`. You can also write this as `1.x`.

### Tilde ranges

> Fixes major and minor numbers. Accept patch changes only.

`~1.2.3` will match all `1.2.x` versions but will miss `1.3.0`.

### Caret ranges

> Fixes the major number only. Accept minor as well as patch changes.

`^1.2.3` will match any `1.x.x` release including `1.3.0`, but will hold off on `2.0.0`.

## npmrc

> Config file.

npm gets its config settings from the command line, environment variables, and npmrc files.

- per-project config file `/path/to/my/project/.npmrc`
- per-user config file `~/.npmrc`
- global config file `$PREFIX/etc/npmrc`
- npm builtin config file `/path/to/npm/npmrc`

## Folder Structures Used by npm

- Local install (default): puts stuff in  `./node_modules`  of the current package root.
- Global install (with  `-g`): puts stuff in /usr/local or wherever node is installed.
- Install it  **locally**  if you’re going to  `require()`  it.
- Install it  **globally**  if you’re going to run it on the command line.
- If you need both, then install it in both places, or use  `npm link`.
