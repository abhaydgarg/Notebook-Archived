# Disjoint set

> Union Find

In computer science, a disjoint-set data structure (also called a union–find data structure or merge–find set) is a data structure that tracks a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It provides near-constant-time operations (bounded by the inverse Ackermann function) to add new sets, to merge existing sets, and to determine whether elements are in the same set.

Using both path compression and union by rank or size ensures that the amortized time per operation is only $O(\alpha n)$, which is optimal, where $\alpha n$ is the inverse Ackermann function. This function has a value $\alpha n < 5$ for any value of `n`, so the disjoint-set operations take place in essentially constant time.

## Operations

### MakeSet

The MakeSet operation makes a new set by creating a new element with a unique id, a rank of `0`, and a parent pointer to itself. The parent pointer to itself indicates that the element is the representative member of its own set.

### Find

`Find(x)` follows the chain of parent pointers from `x` up the tree until it reaches a root element, whose parent is itself. This root element is the representative member of the set to which `x` belongs, and may be `x` itself.

#### Path compression

Path compression flattens the structure of the tree by making every node point to the root whenever Find is used on it. This is valid, since each element visited on the way to a root is part of the same set. The resulting flatter tree speeds up future operations not only on these elements, but also on those referencing them.

### Union

`Union(x,y)` uses Find to determine the roots of the trees `x` and `y` belong to. If the roots are distinct, the trees are combined by attaching the root of one to the root of the other. If this is done naively, such as by always making `x` a child of `y`, the height of the trees can grow as `O(n)`. To prevent this union by rank or union by size is used.

#### by rank

Union by rank always attaches the shorter tree to the root of the taller tree. Thus, the resulting tree is no taller than the originals unless they were of equal height, in which case the resulting tree is taller by one node.

To implement union by rank, each element is associated with a rank. Initially a set has one element and a rank of zero. If two sets are unioned and have the same rank, the resulting set's rank is one larger; otherwise, if two sets are unioned and have different ranks, the resulting set's rank is the larger of the two. Ranks are used instead of height or depth because path compression will change the trees' heights over time.

## Code

### DisjointSet.js

```js
module.exports = class DisjointSet {
  constructor () {
    this.list = new Map();
    this.rank = new Map();
  }

  getListObject () {
    let list = {};
    this.list.forEach((value, key, map) => {
      list[key] = value;
    });
    return list;
  }

  makeSet (item) {
    this.list.set(item, item);
    this.rank.set(item, 0);
  }

  find (item) {
    // If item is not a root
    if (this.list.get(item) !== item) {
      // Path compression
      this.list.set(item, this.find(this.list.get(item)));
    }
    return this.list.get(item);
  }

  union (itemOne, itemTwo) {
    // Find the root
    let rootOne = this.find(itemOne);
    let rootTwo = this.find(itemTwo);

    // If rootOne and rootTwo are present in same set
    if (rootOne === rootTwo) {
      return;
    }

    // Always attach smaller depth tree under the
    // root of the deeper tree.
    let rootOneRank = this.rank.get(rootOne);
    let rootTwoRank = this.rank.get(rootTwo);
    if (rootOneRank > rootTwoRank) {
      this.list.set(rootTwo, rootOne);
    } else if (rootOneRank < rootTwoRank) {
      this.list.set(rootOne, rootTwo);
    } else {
      this.list.set(rootOne, rootTwo);
      this.rank.set(rootTwo, rootTwoRank + 1);
    }
  }
};
```

### Usage

```js
const DisjointSet = require('../DisjointSet');

module.exports.makeset = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let ds = new DisjointSet();

  arr.forEach(item => {
    ds.makeSet(item);
  });

  console.dir(ds.getListObject(), {depth: null});
};

module.exports.find = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let union = [['A', 'B'], ['C', 'D'], ['D', 'E'], ['D', 'B']];
  let find = 'C';
  let ds = new DisjointSet();

  arr.forEach(item => {
    ds.makeSet(item);
  });

  union.forEach(pair => {
    ds.union(pair[0], pair[1]);
  });

  console.dir(ds.find(find), {depth: null});
};

module.exports.union = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let union = [['A', 'B'], ['C', 'D'], ['D', 'E'], ['D', 'B']];
  let ds = new DisjointSet();

  arr.forEach(item => {
    ds.makeSet(item);
  });

  union.forEach(pair => {
    ds.union(pair[0], pair[1]);
  });

  console.dir(ds.getListObject(), {depth: null});
};
```
