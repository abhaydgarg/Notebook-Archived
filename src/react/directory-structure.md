# Directory structure

> **Modularization by feature.**

You can also use **Boilerplate or Toolchain.**

## `index.js` hell

1. Complex and deep dir structure which import component using `index.js` file. This makes the import statements intuitive but make the dir structure complex. Also while development, having all `index.js` files opened in editor make development confusing because you cannot identify the file just by looking its name, you have to look into the file contant.
2. Keep dir structure simple and append the type at the end of file name. For example, `HeaderStyles.js`, `HeaderStory.js`, for component you can avoid appending `component`. Just simply name it `Header.js`. For Containers/Screens `ProfileContainer.js/ProfileScreen.js`, Sagas `ProfileSaga.js`, Navigation `ProfileNavigation.js`, Redux `ProfileRedux.js`, Config `ProfileConfig.js` append type. It makes you to identify the file just by looking its name and avoid name clashing. This way you can also have file name and class name similar.

> Recommended: Do not make development environment complex and less intuitive just because something looks good to human eyes. Compiler does not care. **Go for simple dir structure and do not use `index.js` file unnecessay. use it where appropriate.** You can create a `index.js` file and import all children in it then export them in object.
