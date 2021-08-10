# Heap sort [unstable]

## Complexity

| Algo  | Average    | Worst      |
|-------|------------|------------|
| Sort  | O(n log n) | O(n log n) |
| Space |            | O(1)       |

## Code

### HeapSort.js

```js
module.exports = class HeapSort {
  constructor (list) {
    this.list = list;
    this.order = 'asc';
    this.heapSize = this.list.length;
  }

  getList () {
    return this.list;
  }

  setOrder (order) {
    this.order = order;
  }

  sort () {
    // Build heap.
    this.buildHeap();
    // Find last index.
    let length = this.list.length - 1;
    // Continue heap sorting until we have
    // just one element left in the array.
    for (var index = length; index >= 0; index--) {
      this.swap(0, index);
      this.heapSize--;
      if (this.order === 'desc') {
        this.minHeapify(0);
      } else {
        this.maxHeapify(0);
      }
    }
  }

  buildHeap () {
    // We only need to process the first half of the array
    // because all element will be checked during heapify
    let index = Math.floor(this.list.length / 2 - 1);
    while (index >= 0) {
      if (this.order === 'desc') {
        this.minHeapify(index);
      } else {
        this.maxHeapify(index);
      }
      index--;
    }
  }

  maxHeapify (index) {
    let max = index;
    let left = null;
    let right = null;
    while (index < this.heapSize) {
      left = 2 * index + 1;
      right = left + 1;
      if (left < this.heapSize && this.list[left] > this.list[index]) {
        max = left;
      }
      if (right < this.heapSize && this.list[right] > this.list[max]) {
        max = right;
      }
      if (max === index) {
        return;
      }
      this.swap(index, max);
      index = max;
    }
  }

  minHeapify (index) {
    let min = index;
    let left = null;
    let right = null;
    while (index < this.heapSize) {
      left = 2 * index + 1;
      right = left + 1;
      if (left < this.heapSize && this.list[left] < this.list[index]) {
        min = left;
      }
      if (right < this.heapSize && this.list[right] < this.list[min]) {
        min = right;
      }
      if (min === index) {
        return;
      }
      this.swap(index, min);
      index = min;
    }
  }

  swap (i, j) {
    let tmp = this.list[i];
    this.list[i] = this.list[j];
    this.list[j] = tmp;
  }
};
```

### Usage

```js
const HeapSort = require('./HeapSort');

module.exports.asc = function () {
  let arr = [7, 5, 2, 4, 0, 3, 9, -1];
  let hs = new HeapSort(arr);
  hs.sort();

  console.dir(hs.getList(), {depth: null});
};

module.exports.desc = function () {
  let arr = [7, 5, 2, 4, 0, 3, 9, -1];
  let hs = new HeapSort(arr);
  hs.setOrder('desc');
  hs.sort();

  console.log(hs.getList(), {depth: null});
};
```
