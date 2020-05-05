# File structure

[Atomic design](https://bradfrost.com/blog/post/atomic-web-design/)

## Group (modularize) by _Feature_

One common way to structure projects is locate CSS, JS, and tests together inside folders grouped by feature.

```
common/
  Avatar.js
  Avatar.css
  APIUtils.js
  APIUtils.test.js
feed/
  index.js
  Feed.js
  Feed.css
  FeedStory.js
  FeedStory.test.js
  FeedAPI.js
profile/
  index.js
  Profile.js
  ProfileHeader.js
  ProfileHeader.css
  ProfileAPI.js
```

## Group (modularize) by _File Type_

Another popular way to structure projects is to group similar files together, for example:

```
api/
  APIUtils.js
  APIUtils.test.js
  ProfileAPI.js
  UserAPI.js
components/
  Avatar.js
  Avatar.css
  Feed.js
  Feed.css
  FeedStory.js
  FeedStory.test.js
  Profile.js
  ProfileHeader.js
  ProfileHeader.css
```

## `index.js` hell (Avoid too much nesting)

1. Complex and deep dir structure which import component using `index.js` file. This makes the import statements intuitive but make the dir structure complex. Also while development, having all `index.js` files opened in editor make development confusing because you cannot identify the file just by looking its name, you have to look into the file contant.
2. Keep dir structure simple and append the type at the end of file name. For example, `HeaderStyles.js`, `HeaderStory.js`, for component you can avoid appending `component`. Just simply name it `Header.js`. For Containers/Screens `ProfileContainer.js/ProfileScreen.js`, Sagas `ProfileSaga.js`, Navigation `ProfileNavigation.js`, Redux `ProfileRedux.js`, Config `ProfileConfig.js` append type. It makes you to identify the file just by looking its name and avoid name clashing. This way you can also have file name and class name similar.

> Recommended: Do not make development environment complex and less intuitive just because something looks good to human eyes. Compiler does not care. **Go for simple dir structure and do not use `index.js` file unnecessay. use it where appropriate.** You can create a `index.js` file and import all children in it then export them in object.
