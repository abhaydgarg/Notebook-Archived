# Misc

## Formik - should know the way it handle form's state

It uses the same pattern that you are very well familiar with, which is pass the function as prop to children. For example, you have a parent comp and function called `hello` which update the parent comp state. Now you want that the children can also have an access to `hello` function and they can also trigger state change. To do this we simply pass the `hello` function to child comp as prop `<Child onHello={hello} />`. Now child can run the `hello` using `props.hello()`.

Formik uses the same pattern and let child comp to update form's state.

- It provided the inerface (a set of functions) such as `setFieldValue`, `setFieldTouched`, `setError`, etc.
- The interface is available to child comp where we create our form.
- With the help of functions we can control the form by simply calling these functions and the tedious work of handling the state change is done by Formik automatically. All you need to do is to call the function.

## &&

```jsx
// Renders a <Header /> if showHeader is true
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

## Keys

!!! info
    If key is updated or changed then React remount the component. This is one of the ways when you can use to remount the component instead update the component by update cycle.

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

```js
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

### Choosing a key

- The element you are going to display may already have a unique ID, so the key can just come from your data. If that not the case, you can add a new ID property to your model if it does not present.
- As a last resort, you can pass an item’s index in the array as a key. **This can work well if the items are never reordered, but reorders will be slow.** Reorders can also cause issues with component state when indexes are used as keys.

> Do not generate keys with Math.random(). It creates unstable keys, and will cause many component instances and DOM nodes to be unnecessarily recreated.

## If a parent component is triggered update then does React update all the children

> Yes, Update cycle move down from parent to children.

> Similarily, when parent mount then all its children also run mount cycle.

If parent component runs a update cycle then all children also run the update cycle. Because it is possible that children are depended upon the `state` or `props` of parent component.

**NOTE:** Even if you are not using parent `state` or `prop` in any children component, then also children components run update cycle when state change. For example:

```js
class Parent extends Component {
  constructor (props) {
    super(props);
    this.state = {
      refresh: false
    };
  }

  refresh = () => {
    this.setState({
      refresh: true
    });
  }

  // refresh state variable is not being used
  // in any children components.
  render() {
    return (
      <Fragment>
        <Child1 />
        <Child3>
          {/* more nested children*/}
        </Child3>
        <Child2 />
      </Fragment>
    );
  }
```

In example above `refresh` state property is not being used in any children components. See the `render` method. Now if `refresh()` method is triggered then it execute `setState`, updating the `refresh` property, which tells React to run update cycle. In this case parent component as well as all the childrens run update cycle.

### Now, Does React update the DOM too, when update cycle run?

React is smart enough not to update the actual DOM if nothing seem to be changed in child component, even when the update is triggered down by parent component update. Instead of creating all DOM nodes from scratch and putting them on the page, React will start the reconciliation (or “diffing”) algorithm to determine which parts of the node tree have to be updated and which can be left untouched.

But still the update lifecycle methods do execute in child component, such as `getDerivedStateFromProps` and `componentDidUpdate`.

Therefore, it is said to run logic in `getDerivedStateFromProps` and `componentDidUpdate` using `if` statement, where we must check if `prop` is changed then only run the logic. If condition is missing then logic will run even when nothing has chnaged in child component.

!!! question "What is reconciliation?"
    When a component’s props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM. If they are equal then React does not update the DOM. This process is called reconciliation.
