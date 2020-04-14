# Stack

A stack stores items in a last-in, first-out **LIFO** order.

## Implementation

You can implement a stack with either a linked list or a dynamic array, they both work pretty well.

## Complexity

| Algo | Worst | how come |
|--|--|--|
| Search | O(n) | To find an element you have to start from the first element and look each element one by one. It is possible that the element you are looking for is a last element in the list.
| Insert | O(1) | Insertion is done at front. |
| Delete | O(1) | Deletion is done at front. |

## Code

### Node.js

```js
module.exports = class Node {
  constructor (data) {
    this.data = data;
    this.next = null;
  }
};
```

### Stack.js

```js
const Node = require('./Node');

/**
 * Add    = Push = At the front of list.
 * Remove = Pop = At the front of list.
 */

module.exports = class Stack {
  constructor () {
    this.head = null;
    this.length = 0;
  }

  push (element) {
    let node = new Node(element);
    if (this.head === null) {
      this.head = node;
      this.length = 1;
    } else {
      node.next = this.head;
      this.head = node;
      this.length++;
    }
  }

  pop () {
    if (this.head === null) {
      return null;
    }
    let data = this.head.data;
    this.head = this.head.next;
    this.length--;
    return data;
  }

  peek () {
    if (this.head === null) {
      return null;
    }
    return this.head.data;
  }

  length () {
    let current = this.head;
    let counter = 0;
    while (current !== null) {
      current = current.next;
      counter++;
    }
    return counter;
  }

  isEmpty () {
    return this.length <= 0;
  }
};
```
