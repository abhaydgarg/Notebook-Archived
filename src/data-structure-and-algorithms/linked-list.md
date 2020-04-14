# Linked list

A linked list organizes items sequentially, with each item storing a pointer to the next one.

An item in a linked list is called a **node**. The first node is called the **head**. The last node is called the **tail**.

> Confusingly, somtimes people use the word tail to refer to "the whole rest of the list after the head."

## Strengths

- **Insert and delete operations on both ends**. Inserting and removing element at either end of a linked list is O(1).
- **Flexible size**. There's no need to specify how many elements you're going to store ahead of time. You can keep adding elements as long as there's enough space on the machine.

## Weaknesses

- **Costly lookups**. To search an item in a linked list, you have to take O(n) time.

## Uses

Stacks and queues only need fast operations on the ends, so linked list is ideal.

## Complexity

| Algo   | Worst | how come                                                                                                                                                                |
|--------|-------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Search | O(n)  | To find an element you have to start from the head and look each element one by one. It is possible that the element you are looking for is a last element in the list. |
| Insert | O(1)  | Inserting at the either ends.                                                                                                                                           |
| Delete | O(1) | Deleting at the either ends.

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

### LinkedList.js

```js
const Node = require('./Node');

module.exports = class LinkedList {
  constructor () {
    this.head = null;
    this.tail = null;
  }

  push (data) {
    let node = new Node(data);
    if (this.head === null) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      this.tail = node;
    }
  }

  pop () {
    // pop is expensive because it require
    // traversing list to the end.
    // Double linked list - is a great
    // option when you want to remove
    // node from end of the list.

    if (this.head === null) {
      return null;
    }

    if (this.head.next === null) {
      let temp = this.head;
      this.head = null;
      this.tail = null;
      return temp;
    }

    let temp = this.tail;
    let current = this.head;
    let prev = null;
    while (current !== null) {
      if (current.next === null) {
        this.tail = prev;
        prev.next = null;
        break;
      }
      prev = current;
      current = current.next;
    }
    return temp;
  }

  unshift (data) {
    let node = new Node(data);
    if (this.head === null) {
      this.head = node;
      this.tail = node;
    } else {
      let temp = this.head;
      this.head = node;
      this.head.next = temp;
    }
  }

  shift () {
    if (this.head === null) {
      return null;
    }

    let temp = this.head;
    this.head = this.head.next;
    temp.next = null;
    if (this.head === null) {
      this.tail = null;
    }
    return temp;
  }

  add (node, data) {
    let newNode = new Node(data);
    if (node.next === null) {
      node.next = newNode;
      this.tail = newNode;
    } else {
      newNode.next = node.next;
      node.next = newNode;
    }
  }

  remove (node) {
    if (this.head === node && node.next === null) {
      this.head = null;
      this.tail = null;
      return;
    }
    if (this.head === node && node.next !== null) {
      this.head = node.next;
      return;
    }
    let current = this.head.next;
    let prev = this.head;
    while (current !== node) {
      prev = current;
      current = current.next;
    }
    prev.next = current.next;
    // update tail if last element is deleted
    if (prev.next === null) {
      this.tail = prev;
    }
  }

  removeAtPosition (position) {
    if (position === 0 && this.head.next === null) {
      this.head = null;
      this.tail = null;
      return;
    }
    if (position === 0 && this.head.next !== null) {
      this.head = this.head.next;
      return;
    }
    let current = this.head.next;
    let prev = this.head;
    let counter = 1; // 0 position is aleady handled above.
    while (position !== counter) {
      prev = current;
      current = current.next;
      counter++;
    }
    prev.next = current.next;
    // update tail if last element is deleted
    if (prev.next === null) {
      this.tail = prev;
    }
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

  lengthByRecursion (current = this.head) {
    if (current === null) {
      return 0;
    }
    return 1 + this.lengthByRecursion(current.next);
  }

  find (element) {
    let current = this.head;
    while (current !== null) {
      if (current.data === element) {
        return true;
      }
      current = current.next;
    }
    return false;
  }

  findMiddle () {
    // Traverse linked list using two pointers.
    // Move one pointer by one and other pointer by two.
    // When the fast pointer reaches end, slow pointer will
    // reach middle of the linked list.
    if (this.head === null) {
      return null;
    }
    let current = this.head;
    let lookAhead = this.head;
    while (lookAhead !== null && lookAhead.next !== null) {
      current = current.next;
      lookAhead = lookAhead.next.next;
    }
    return current.data;
  }

  reverse () {
    if (this.head === null) {
      return null;
    }
    let current = this.head;
    let prev = null;
    let next = null;
    while (current !== null) {
      next = current.next;
      current.next = prev;
      if (prev === null) {
        this.tail = current;
      }
      prev = current;
      current = next;
    }
    this.head = prev;
  }
};
```

### Usage

```js
// Create a linked list
module.exports.create = function () {
  let arr = [100, 200, 300, 400, 500];
  let linkedList = new LinkedList();
  for (let i = 0; i < arr.length; i++) {
    linkedList.push(arr[i]);
  }
  console.dir(linkedList, {depth: null});
}
```
