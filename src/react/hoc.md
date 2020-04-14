# HOC

> Used to reuse component logic. It is a pattern used to share component functionality between components without repeating code.

Higher-Order Components (or HOCs) in React are functions that take a component and return a new component, **enhancing the original in some way:**

```js
const EnhancedComponent = hoc(OriginalComponent);
```

Two HOC’s implementations that you may be familiar with in the React ecosystem are `connect` from Redux and `withRouter` from React Router. The `connect` function from Redux is used to give components access to the global state in the Redux store, and it passes these values to the component as props. The `withRouter` function injects the router information and functionality into the component, enabling the developer access or change the route.

HOC will always have a form similar to the follow:

```js
const higherOrderComponent = (WrappedComponent) => {
  class HOC extends React.Component {
    render() {
      return <WrappedComponent />;
    }
  }

  return HOC;
};
```

## Example

`withStorage` high order function that we can use to store the data in localstorage. The idea is that we create high order function `withStorage` which has couple of methods that can be used to perform various localstorage operations. We are doing this because many different components want to store the information in localstorage and we want to keep the logic of accessing, removing and updating localstorge at one place and resuse it where needed.

!!! note
    You can accomplish this by creating localstorage util class having mehtods required to operate on localstorage and use them in any component by importing the util class. But we want to do it in HOC way.

```js
const withStorage = (WrappedComponent) => {
  class HOC extends React.Component {

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
        <WrappedComponent
          load={this.load}
          save={this.save}
          remove={this.remove}
          {...this.props} // Passing props of WrappedComponent
        />
      );
    }
  }

  return HOC;
}

export default withStorage;
```

At the top of `withStorage` we have a single item in the component’s state which tracks if `localStorage` is available in the given browser. We use the `componentDidMount` lifecycle hook which will check if localStorage exists in the `checkLocalStorageExists` function. Here it will test saving an item and set the state to true if it succeeds.

We also add three functions to our HOC — `load`, `save`, and `remove`. These are used to directly access the `localStorage` API if it is available. Our three functions on the HOC are passed to our wrapped component.

```js
import withStorage from '../withStorage';

class ComponentUsingStorage extends React.Component {
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

export default withStorage(ComponentUsingStorage);
```

Inside the `componentDidMount` of our wrapped component, we first try to access the `username` and `favoriteMovie` from `localStorage`. If the values do not exist, we make our expensive API call named `apiCall`. Once this function returns, we save the username and favorite to `localStorage` and update the component’s state to display them on the screen.

## Considerations

1. It should not make any modifications and just compose the original component by enhancing it some way. Think of it as **open/close principle** where module is open for extension but close for modification.
2. Do not use HOC’s in the **render method** of a component. Access the HOC outside the component definition.
3. **Static methods** must be copied over to still have access to them. A simple way to do this is the hoist-non-react-statics package.
4. **Refs** are not passed through.
