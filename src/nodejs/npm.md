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

## npm install

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

## npmrc

> Config file.

npm gets its config settings from the command line, environment variables, and npmrc files.

- per-project config file `/path/to/my/project/.npmrc`
- per-user config file `~/.npmrc`
- global config file `$PREFIX/etc/npmrc`
- npm builtin config file `/path/to/npm/npmrc`

## package.json - worthy to know some properties

### Another ways to add package in package.json

```js
// URL - tarball
"http://asdf.com/asdf.tar.gz"

// URL - git
"git+ssh://git@github.com:npm/cli.git#v1.0.27"
"git+ssh://git@github.com:npm/cli#semver:^5.0"
"git+https://isaacs@github.com/npm/cli.git"
"git://github.com/npm/cli.git#v1.0.27"

// Local path
// Run
npm install <localpath>
// This save the package with relative path
"file:../foo/bar"
```

### Three ways to add dependencies

- dependencies
- devDependencies
- peerDependencies

### engines

Specify the version of **node** that your stuff works on. If you don’t specify the version (or if you specify “*” as the version), then any version of node will do.

```js
{ "engines" : { "node" : "10" } }
```

### private

If you set `"private": true` in your package.json, then npm will refuse to publish it. This is a way to prevent accidental publication of private repositories.

## Folder Structures Used by npm

- Local install (default): puts stuff in  `./node_modules`  of the current package root.
- Global install (with  `-g`): puts stuff in /usr/local or wherever node is installed.
- Install it  **locally**  if you’re going to  `require()`  it.
- Install it  **globally**  if you’re going to run it on the command line.
- If you need both, then install it in both places, or use  `npm link`.

### Executables (bin)

When in global mode, executables are linked into  `{prefix}/bin`  on Unix, or directly into  `{prefix}`  on Windows.

When in local mode, executables are linked into  `./node_modules/.bin`  so that they can be made available to scripts run through npm.

### cache

Cache files are stored in `~/.npm` on Posix, or `%AppData%/npm-cache` on Windows.

## package-lock.json

Let’s say we create a new project that is going to use express. After running `npm init`, we install express: `npm install express`. At the time of writing, the latest express version is `4.15.4`. So “express”: `^4.15.4` is added as a dependency within my package.json and that exact version is installed on my machine. Now maybe tomorrow, the maintainers of express release a bug fix, and so the latest version becomes `4.15.5`. Then if someone were to want to contribute to my project, they would clone it, and run `npm install.` Since `4.15.5` is a higher version with the same major version, that is installed for them. We both have express, but we have two different versions. Theoretically, they should still be compatible, but maybe that bugfix affected functionality that we are using, and our application would produce different results when run with express version `4.15.4` compared to `4.15.5`.

package-lock.json will simply avoid this general behavior of installing updated minor version so when someone clones your repo and run npm install in their machine. NPM will look into package-lock.json and install exact versions of the package as the owner has installed.

!!! info "The story about package.json vs package-lock.json is tricky"
    `npm install` verify that the package.json and package-lock.json _correspond to each other_. That is, if the semver versions described in package.json fit with the locked versions in package-lock.json, `npm install` will use the package-lock.json.

    Now, if you _change_  `package.json` such that the versions in `package-lock.json` are no longer valid, your `npm install` will be treated as if you'd done `npm install some-pkg@x.y.z`, where `x.y.z` is the new version in the package.json. In this case of conflict, the version in package.json file is installed.

    The conflict happens when someone directly edit the package.json file and does not run `npm install` and ship the package.

!!! note
    Always commit package-lock.json file

## npm ci

As mentioned above in package.json, when version conflict is occured between package.json and package-lock.json then version in pakcage.json is installed.

But what if you want to notify the user about the conflict?

- `npm ci` can only install entire projects at a time: individual dependencies cannot be added with this command.
- If dependencies in the package-lock.json do not match those in package.json, `npm ci` will exit with an error, instead of updating the package lock.
- If a node_modules is already present, it will be automatically removed before `npm ci` begins its install.
- It will never write to package.json or any of the package-locks: installs are essentially frozen.

## npm-shrinkwrap.json

It lets you lock down the version numbers all the packages and their descendant packages in your `node_modules` directory.

The issue arises due to the way npm install works. See npm install save the entry in `package.json` using caret `^` which means if you have entry say `"lodash": "^1.2.9"` in `package.json` and you hand the code over to some person and when he run `npm install`, there is new version lodash version available `1.3.0`, so in this case npm does not install `1.2.9` but `1.3.0`. We can control this by setting default config by running `npm config set save-exact true` which save the entry without `^`. However that only solves the problem for the direct dependents of your package. It does not give you control over the installed versions of the deeply nested packages that are the dependencies of your dependencies and beyond.

This can be crucial to you in a production environment because you need to ensure that each production re-deployment always always installs the same versions of the package as the other deployments.

This is where `npm shrinkwrap` comes into play. When you run `npm shrinkwrap` in a project after running `npm install`, it creates a file called `npm-shrinkwrap.json` which lists the exact package versions of all the installed packages in the entire hierarchy.

Also note that by default npm shrinkwrap does not include your devDependencies unless you pass run `npm shrinkwrap --dev`.

## npmx

Let's know something about `npm` first.

One might install a package locally on a certain project:

```bash
npm install some-package
```

Now let's say you want NodeJS to execute that package from the command line:

```bash
$ some-package
```

The above will fail. Only globally installed packages can be executed by typing their name only.

To fix this, and have it run, you must type the local path:

```bash
$ ./node_modules/.bin/some-package
```

You can technically run a locally installed package by editing your packages.json file and adding that package in the `scripts` section:

```js
{
  "name": "whatever",
  "version": "1.0.0",
  "scripts": {
    "some-package": "some-package"
  }
}
```

Then run the script using `npm run-script` (or `npm run`):

```bash
npm run some-package
```

### To avoid all this hassles, `npx` is introduced

#### [1] Package is already installed

Package is already installed and you want to run package execuatable inside `/node_modules/.bin/`.

`npx` will check whether `<command>` exists in `$PATH`, or in the local project binaries, and execute it. So, for the above example, if you wish to execute the locally-installed package `some-package` all you need to do is type:

```bash
npx some-package
```

#### [2] Package is not already installed

Another major advantage of npx is the ability to execute a package which wasn't previously installed:

```bash
$ npx create-react-app my-app
```

In example above, `create-react-app` package is downloaded temporarily, create the react app and when finished the package is removed. `create-react-app` is an npm package that is expected to be run only once in a project's lifecycle. Hence, it is preferred to use npx to install and run it in a single step.
