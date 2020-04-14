# Binary search

In computer science, binary search, also known as `half-interval` search, `logarithmic` search, or `binary chop`, is a search algorithm that finds the position of a target value within a `sorted` array. Binary search compares the target value to the middle element of the array; if they are unequal, the half in which the target cannot lie is eliminated and the search continues on the remaining half until it is successful. If the search ends with the remaining half being empty, the target is not in the array.

## Complexity

O(log n) - since we split search area by two for every next iteration.

## Code

### BinarySearch.js

```js
module.exports = class BinarySearch {
  constructor (list) {
    this.list = list;
  }

  find (target) {
    let left = 0;
    let right = this.list.length - 1;
    while (left <= right) {
      let mid = left + Math.floor((right - left) / 2);
      if (this.list[mid] === target) {
        return true;
      }
      if (this.list[mid] < target) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }
    return false;
  }
};
```
