# Queue

A queue stores items in a first-in, first-out **FIFO** order.

## Implementation

Queues are easy to implement with linked lists. To enqueue, insert at the tail of the linked list. To dequeue, remove at the head of the linked list.

| Algo | Worst | how come |
|--|--|--|
| Search | O(n) | To find an element you have to start from the first element and look each element one by one. It is possible that the element you are looking for is a last element in the list.
| Insert | O(1) | Insertion is done at end. |
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

### Queue.js

```js
const Node = require('./Node');

/**
 * Add    = Enqueue = At the end of list.
 * Remove = Dequeue = At the front of list.
 */

module.exports = class Queue {
  constructor () {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }

  enqueue (element) {
    let node = new Node(element);
    if (this.head === null) {
      this.head = node;
      this.tail = node;
      this.length = 1;
    } else {
      this.tail.next = node;
      this.tail = node;
      this.length++;
    }
  }

  dequeue () {
    if (this.head === null) {
      return null;
    }
    let data = this.head.data;
    this.head = this.head.next;
    if (this.head === null) {
      this.tail = null;
    }
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
