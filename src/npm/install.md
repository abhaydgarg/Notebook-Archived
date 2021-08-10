# npm install

`npm install <package_name>@<version_number>`

```bash
npm install lodash@latest // For the last stable version
npm install lodash@next // For the most recent release

npm install lodash@4.17.14 // For the exact release
// OR
npm install lodash@=4.17.14

// Ranges can also be used.
npm install lodash@"<=3" // Use of quotes
npm install sax@">=0.1.0 <0.2.0"
```

- By default save the package to `dependencies` property of `package.json`. No need to mention `--save || -S` flag.
- Use `-D || --save-dev` to list the package to `devDependencies`.
- Use `-E || --save-exact` to save the package without `^` in `package.json`.

> By default npm use caret `^` in front of package version when save in `package.json`. If you do not want `^` then use `-E || --save-exact` flag.

```bash
// Save as: "loadash": "^4.17.14"
npm install lodash

// Save as: "loadash": "4.17.14"
npm install -E lodash
```

## Change npm default `^`

When you install package, npm by default save it in the `package.json` file with caret `^` in front of version. The default behavior can be altered.

`npm config set save-prefix '~'`

`npm config set save-exact true`
