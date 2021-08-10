# Selection sort [unstable]

Go through the array, find the index of the lowest element swap the lowest element with the first element. Hence first element is the lowest element in the array.

Now go through the rest of the array (excluding the first element) and find the index of the lowest and swap it with the second element.

thats how it continues to select (find out) the lowest element of the array and putting it on the left until it hits the last element.

## Complexity

| Algo  | Average | Worst   |
|-------|---------|---------|
| Sort  | O(n^2^) | O(n^2^) |
| Space |         | O(1)    |

## Code

### SelectionSort.js

```js
module.exports = class SelectionSort {
  constructor (list) {
    this.list = list;
  }

  getList () {
    return this.list;
  }

  asc () {
    let minIndex = null;
    let temp = null;
    for (var i = 0; i < this.list.length; i++) {
      minIndex = i;
      for (var j = i + 1; j < this.list.length; j++) {
        if (this.list[j] < this.list[minIndex]) {
          minIndex = j;
        }
      }
      temp = this.list[i];
      this.list[i] = this.list[minIndex];
      this.list[minIndex] = temp;
    }
  }

  desc () {
    let maxIndex = null;
    let temp = null;
    for (var i = 0; i < this.list.length; i++) {
      maxIndex = i;
      for (var j = i + 1; j < this.list.length; j++) {
        if (this.list[j] > this.list[maxIndex]) {
          maxIndex = j;
        }
      }
      temp = this.list[i];
      this.list[i] = this.list[maxIndex];
      this.list[maxIndex] = temp;
    }
  }
};
```
