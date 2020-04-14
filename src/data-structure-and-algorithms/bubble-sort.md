# Bubble sort [stable]

Bubble sort algorithm starts by comparing the first two elements of an array and swapping if necessary, i.e., if you want to sort the elements of array in ascending order and if the first element is greater than second then, you need to swap the elements but, if the first element is smaller than second, you mustn't swap the element. Then, again second and third elements are compared and swapped if it is necessary and this process go on until last and second last element is compared and swapped. This completes the first step of bubble sort.

If there are `n` elements to be sorted then, the process mentioned above should be repeated `n-1` times to get required result. But, for better performance, in second step, last and second last elements are not compared becuase, the proper element is automatically placed at last after first step. Similarly, in third step, last and second last and second last and third last elements are not compared and so on.

## Complexity

| Algo  | Average | Worst   |
|-------|---------|---------|
| Sort  | O(n^2^) | O(n^2^) |
| Space |         | O(n)    |

## Code

### BubbleSort.js

```js
module.exports = class BubbleSort {
  constructor (list) {
    this.list = list;
  }

  getList () {
    return this.list;
  }

  asc () {
    let temp = null;
    for (var iteration = 0; iteration < this.list.length; iteration++) {
      for (var i = 0; i < this.list.length - (iteration + 1); i++) {
        if (this.list[i] > this.list[i + 1]) {
          temp = this.list[i];
          this.list[i] = this.list[i + 1];
          this.list[i + 1] = temp;
        }
      }
    }
  }

  desc () {
    let temp = null;
    for (var iteration = 0; iteration < this.list.length; iteration++) {
      for (var i = 0; i < this.list.length - (iteration + 1); i++) {
        if (this.list[i] < this.list[i + 1]) {
          temp = this.list[i];
          this.list[i] = this.list[i + 1];
          this.list[i + 1] = temp;
        }
      }
    }
  }
};
```
