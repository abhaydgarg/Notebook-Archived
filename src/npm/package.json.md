# package.json - worthy to know some properties

## Another ways to add package in package.json

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

## Three ways to add dependencies

- dependencies
- devDependencies
- peerDependencies

## engines

Specify the version of **node** that your stuff works on. If you don’t specify the version (or if you specify “*” as the version), then any version of node will do.

```js
{ "engines" : { "node" : "10" } }
```

## private

If you set `"private": true` in your package.json, then npm will refuse to publish it. This is a way to prevent accidental publication of private repositories.

## Executables (bin)

When in global mode, executables are linked into  `{prefix}/bin`  on Unix, or directly into  `{prefix}`  on Windows.

When in local mode, executables are linked into  `./node_modules/.bin`  so that they can be made available to scripts run through npm.

## cache

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
