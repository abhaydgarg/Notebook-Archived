# Nary tree

## Code

### Node.js

```js
module.exports = class Node {
  constructor (data) {
    this.data = data;
    this.children = [];
  }
};
```

### NaryTree.js

```js
module.exports = class NaryTree {
  constructor () {
    this.root = null;
    this.container = [];
  }

  getTree () {
    return this.root;
  }

  setTree (root) {
    this.root = root;
  }

  getContainer () {
    return this.container;
  }

  bfs () {
    let queue = [];
    let current = null;
    queue.push(this.root);
    while (queue.length > 0) {
      current = queue.shift();
      this.container.push(current.data);
      if (current.children !== undefined && current.children.length > 0) {
        for (let i = 0; i < current.children.length; i++) {
          queue.push(current.children[i]);
        }
      }
    }
  }

  preorder () {
    let stack = [];
    let current = null;
    stack.push(this.root);
    while (stack.length > 0) {
      current = stack.pop();
      this.container.push(current.data);
      if (current.children !== undefined && current.children.length > 0) {
        // We must store children from right to left so that leftmost node is popped first.
        for (let i = current.children.length - 1; i >= 0; i--) {
          stack.push(current.children[i]);
        }
      }
    }
  }

  preorderRecursively (root = this.root) {
    if (root !== null) {
      this.container.push(root.data);
      if (root.children !== undefined && root.children.length > 0) {
        for (let i = 0; i < root.children.length; i++) {
          this.preorderRecursively(root.children[i]);
        }
      }
    }
    return null;
  }
};
```
