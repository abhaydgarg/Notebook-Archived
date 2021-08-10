# Render prop

> Like HOC, it is a pattern to share component functionality with other components without duplicating the code.

The term “render prop” refers to a simple technique for sharing code between React components using a `prop` whose value is a `function`.

> render prop is a function.

```js
<SomeComponent render={data => (
  <h1>Hello {data}</h1>
)}/>
```

## Example

We are going to take same example that we have used in HOC which is reusing the localstorage functionality across other components.

```js
export default class Storage extends React.Component {

  constructor() {
    super();
    this.state = {
      localStorageAvailable: false
    };
  }

  componentDidMount() {
    this.checkLocalStorageExists();
  }

  checkLocalStorageExists() {
    const testKey = 'test';
    try {
      localStorage.setItem(testKey, testKey);
      localStorage.removeItem(testKey);
      this.setState({ localStorageAvailable: true });
    } catch(e) {
      this.setState({ localStorageAvailable: false });
    }
  }

  load = (key) => {
    if (this.state.localStorageAvailable) {
      return localStorage.getItem(key);
    }
    return null;
  }

  save = (key, data) => {
    if (this.state.localStorageAvailable) {
      localStorage.setItem(key, data);
    }
  }

  remove = (key) => {
    if (this.state.localStorageAvailable) {
      localStorage.removeItem(key);
    }
  }

  render() {
    return (
      this.props.render({
        load: this.load,
        save: this.save,
        remove: this.remove
      });
    );
  }
}
```

Now we will create a new render component to utilize in our shared `<Storage />` component which allows it to gain the storage functionality.

```js
export default class ComponentUsingStorage extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      favoriteMovie: ''
    };
  }

  componentDidMount() {
    const username = this.props.load('username');
    const favoriteMovie = this.props.load('favoriteMovie');

    if (!username || !favoriteMovie) {
      apiCall().then((user) => {
        // Using methods of HOC
        this.props.save('username', user.username);
        this.props.save('favoriteMovie', user.favoriteMovie);
        this.setState({
          username: user.username,
          favoriteMovie: user.favoriteMovie,
        });
      });
    }
  }

  render() {
    if (!username || !favoriteMovie) {
      return <div>Loading...</div>;
    }
    return (
      <div>
        My username is {username}, and I love to watch {favoriteMovie}.
      </div>
    );
  }
}
```

```js
import Storage from '../Storage';
import ComponentUsingStorage from './ComponentUsingStorage';

class SomeComponent extends React.Component {
  render() {
    return (
      <Storage
        render={({ load, save, remove }) => {
          return (
            <ComponentUsingStorage
              load={load}
              save={save}
              remove={remove}
            />
          )
        }}
      />
    );
  }
}
```

!!! note
    It’s up to the developer to decide which method they prefer in render props vs HOC. It is my personal opinion that render props work very well for read-only operation like tracking the scroll position or mouse location on the screen. I believe that HOC’s tend to work better for more complex operations such as our `localStorage` functionality.
