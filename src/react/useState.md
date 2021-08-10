# useState

```js
const [count, setCount] = useState(0);

// Update a state variable count
setCount(count + 1)
```

- `count` = state variable.
- `setCount` => `this.setState`. It is **setter** to update the state variable `count`.
- `0` is a initial value of `count`.

## State variable can be of any type

```js
// Number.
const [age, setAge] = useState(42);
// String.
const [fruit, setFruit] = useState('banana');
// Object.
const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
```

## Multiple useState are allowed in a function

```js
const [name, setName] = useState();
const [surname, setSurname] = useState();
const [age, setAge] = useState();
```

## Using previous state equivalent to `this.setSate((prevState, props) { })`

```js
setCount(prevCount => prevCount - 1)
```
