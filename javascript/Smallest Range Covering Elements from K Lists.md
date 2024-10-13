# Smallest Range Covering Elements from K Lists

You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` or `a < c` if `b - a == d - c`.

**Example 1:**
```
Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
Output: [20,24]
Explanation: 
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
List 2: [0, 9, 12, 20], 20 is in range [20,24].
List 3: [5, 18, 22, 30], 22 is in range [20,24].
```

**Example 2:**
```
Input: nums = [[1,2,3],[1,2,3],[1,2,3]]
Output: [1,1]
```

**Constraints:**

- `nums.length == k`
- `1 <= k <= 3500`
- `1 <= nums[i].length <= 50`
- -10<sup>5</sup> <= nums[i][j] <= 10<sup>5</sup>
- `nums[i]` is sorted in **non-decreasing** order.

### Answer:

```javascript
class MinHeap {
  constructor() {
    this.heap = [];
  }

  insert(element) {
    this.heap.push(element);
    let index = this.heap.length - 1;
    while (index > 0) {
      let parent = Math.floor((index - 1) / 2);
      if (this.heap[index].value >= this.heap[parent].value) break;
      [this.heap[index], this.heap[parent]] = [this.heap[parent], this.heap[index]];
      index = parent;
    }
  }

  extractMin() {
    const min = this.heap[0];
    const end = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = end;
      let index = 0;
      while (true) {
        let left = 2 * index + 1, right = 2 * index + 2, smallest = index;
        if (left < this.heap.length && this.heap[left].value < this.heap[smallest].value) smallest = left;
        if (right < this.heap.length && this.heap[right].value < this.heap[smallest].value) smallest = right;
        if (smallest === index) break;
        [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
        index = smallest;
      }
    }
    return min;
  }
}

function smallestRange(nums) {
  const minHeap = new MinHeap();
  let max = -Infinity, start = 0, end = Infinity;

  nums.forEach((list, i) => {
    minHeap.insert({ value: list[0], listIndex: i, index: 0 });
    max = Math.max(max, list[0]);
  });

  while (true) {
    const min = minHeap.extractMin();
    if (max - min.value < end - start) [start, end] = [min.value, max];
    if (min.index + 1 < nums[min.listIndex].length) {
      const nextValue = nums[min.listIndex][min.index + 1];
      minHeap.insert({ value: nextValue, listIndex: min.listIndex, index: min.index + 1 });
      max = Math.max(max, nextValue);
    } else break;
  }

  return [start, end];
}

// Example usage:
const nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]];
console.log(smallestRange(nums)); // Output: [20, 24]
```