# Commands

## Running multiple bash commands

| Syntax | Description |
|--|--|
| `A ; B` | Run A and then run B |
| `A && B` | Run A, if it succeeds then run B |
| `A || B` | Run A, if it fails then run B |

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
