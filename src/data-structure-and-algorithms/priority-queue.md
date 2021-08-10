# Priority queue

This is queue is just like a regular queue data structure , but unlike normal queue the elements of this queue has certain priority assigned to it. So the elements in the queue will be served according to their priorities.

## Implementation

### Naive Approach

Using an array to insert element at the right position in O(n) time or insert at the end using O(1) and then sort it to maintain a priority in O(n log n) time.

### Efficient approach

We can use heap to implement the priority queue. It will take O(log n) time to insert and delete each element in the priority queue. Based on heap structure, priority queue also has two types **max-priority** queue and **min-priority** queue.

#### Complexity

If implemented using heap then it has time complexity of heap for insert and delete O(log n).

## Code

### Node.js

```js
module.exports = class Node {
  constructor (data, priority) {
    this.data = data;
    this.priority = priority;
  }
};
```

### MinPriorityQueue.js

```js
const Node = require('./Node');

module.exports = class MinPriorityQueue {
  constructor () {
    this.items = [];
  }

  getAll () {
    return this.items;
  }

  enqueue (data, priority) {
    let node = new Node(data, priority);
    this.items.push(node);
    let index = this.items.length - 1;
    let temp = null;
    let parent = null;
    while (index > 0) {
      // get the parent
      parent = Math.floor((index - 1) / 2);
      // if parent is greater than child
      if (this.items[index].priority < this.items[parent].priority) {
        // swap
        temp = this.items[parent];
        this.items[parent] = this.items[index];
        this.items[index] = temp;
      }
      index = parent;
    }
  }

  dequeue () {
    if (this.items.length === 0) {
      return null;
    }
    if (this.items.length === 1) {
      return this.items.pop();
    }
    let min = this.items[0];
    // set first element to last element
    this.items[0] = this.items.pop();
    let index = 0;
    let leftChild = null;
    let rightChild = null;
    let swap = null;
    let temp = null;
    while (true) {
      leftChild = 2 * index;
      rightChild = 2 * index + 1;

      // if current is less than left child
      if (leftChild < this.items.length && this.items[index].priority > this.items[leftChild].priority) {
        swap = leftChild;
      }

      // if right child is greater than left child
      if (rightChild < this.items.length && this.items[leftChild].priority > this.items[rightChild].priority) {
        swap = rightChild;
      }

      if (swap === null) {
        break;
      }

      temp = this.items[swap];
      this.items[swap] = this.items[index];
      this.items[index] = temp;

      index = swap;
      swap = null; // loop breaking condition
    }

    return min;
  }
};
```

### MaxPriorityQueue.js

```js
const Node = require('./Node');

module.exports = class MaxPriorityQueue {
  constructor () {
    this.items = [];
  }

  getAll () {
    return this.items;
  }

  enqueue (data, priority) {
    let node = new Node(data, priority);
    this.items.push(node);
    let index = this.items.length - 1;
    let temp = null;
    let parent = null;
    while (index > 0) {
      // get the parent
      parent = Math.floor((index - 1) / 2);
      // if parent is less than child
      if (this.items[index].priority > this.items[parent].priority) {
        // swap
        temp = this.items[parent];
        this.items[parent] = this.items[index];
        this.items[index] = temp;
      }
      index = parent;
    }
  }

  dequeue () {
    if (this.items.length === 0) {
      return null;
    }
    if (this.items.length === 1) {
      return this.items.pop();
    }
    let max = this.items[0];
    // set first element to last element
    this.items[0] = this.items.pop();
    let index = 0;
    let leftChild = null;
    let rightChild = null;
    let swap = null;
    let temp = null;
    while (true) {
      leftChild = 2 * index;
      rightChild = 2 * index + 1;

      // if current is less than left child
      if (leftChild < this.items.length && this.items[index].priority < this.items[leftChild].priority) {
        swap = leftChild;
      }

      // if right child is greater than left child
      if (rightChild < this.items.length && this.items[leftChild].priority < this.items[rightChild].priority) {
        swap = rightChild;
      }

      if (swap === null) {
        break;
      }

      temp = this.items[swap];
      this.items[swap] = this.items[index];
      this.items[index] = temp;

      index = swap;
      swap = null; // loop breaking condition
    }

    return max;
  }
};
```

### Usage

#### Max priority queue

```js
const MaxPriorityQueue = require('./MaxPriorityQueue');

module.exports.enqueue = function () {
  let items = [
    { data: 'Alpha', priority: 5 },
    { data: 'Beta', priority: 1 },
    { data: 'Gamma', priority: 3 },
    { data: 'Omega', priority: 2 },
    { data: 'Theta', priority: 6 }
  ];

  let queue = new MaxPriorityQueue();
  items.forEach((item) => {
    queue.enqueue(item.data, item.priority);
  });

  console.dir(queue.getAll(), {depth: null});
};

module.exports.dequeue = function () {
  let queue = new MaxPriorityQueue();
  [
    { data: 'Alpha', priority: 5 },
    { data: 'Beta', priority: 1 },
    { data: 'Gamma', priority: 3 },
    { data: 'Omega', priority: 2 },
    { data: 'Theta', priority: 6 }
  ].forEach((item) => {
    queue.enqueue(item.data, item.priority);
  });

  queue.dequeue();

  console.dir(queue.getAll(), {depth: null});
};
```

#### Min priority queue

```js
const MinPriorityQueue = require('./MinPriorityQueue');

module.exports.enqueue = function () {
  let items = [
    { data: 'Alpha', priority: 5 },
    { data: 'Beta', priority: 1 },
    { data: 'Gamma', priority: 3 },
    { data: 'Omega', priority: 2 },
    { data: 'Theta', priority: 6 }
  ];

  let queue = new MinPriorityQueue();
  items.forEach((item) => {
    queue.enqueue(item.data, item.priority);
  });

  console.dir(queue.getAll(), {depth: null});
};

module.exports.dequeue = function () {
  let queue = new MinPriorityQueue();
  [
    { data: 'Alpha', priority: 5 },
    { data: 'Beta', priority: 1 },
    { data: 'Gamma', priority: 3 },
    { data: 'Omega', priority: 2 },
    { data: 'Theta', priority: 6 }
  ].forEach((item) => {
    queue.enqueue(item.data, item.priority);
  });

  queue.dequeue();

  console.dir(queue.getAll(), {depth: null});
};
```
