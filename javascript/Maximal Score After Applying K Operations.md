# Maximal Score After Applying K Operations

You are given a **0-indexed** integer array `nums` and an integer `k`. You have a **starting score** of `0`.

In one **operation**:

1. choose an index `i` such that `0 <= i < nums.length`,
2. increase your **score** by `nums[i]`, and
3. replace `nums[i]` with `ceil(nums[i] / 3)`.

Return *the maximum possible **score** you can attain after applying **exactly** `k` operations.*

The ceiling function `ceil(val)` is the least integer greater than or equal to `val`.

**Example 1:**
```
Input: nums = [10,10,10,10,10], k = 5
Output: 50
Explanation: Apply the operation to each array element exactly once. The final score is 10 + 10 + 10 + 10 + 10 = 50.
```

**Example 2:**
```
Input: nums = [1,10,3,3,3], k = 3
Output: 17
Explanation: You can do the following operations:
Operation 1: Select i = 1, so nums becomes [1,4,3,3,3]. Your score increases by 10.
Operation 2: Select i = 1, so nums becomes [1,2,3,3,3]. Your score increases by 4.
Operation 3: Select i = 2, so nums becomes [1,1,1,3,3]. Your score increases by 3.
The final score is 10 + 4 + 3 = 17.
``` 

**Constraints:**

- 1 <= nums.length, k <= 10<sup>5</sup>
- 1 <= nums[i] <= 10<sup>9</sup>

### Answer:

```javascript
class MaxHeap {
    constructor() {
        this.heap = [];
    }

    // Insert an element into the heap
    insert(val) {
        this.heap.push(val);
        this.bubbleUp(this.heap.length - 1);
    }

    // Remove and return the maximum element from the heap
    extractMax() {
        if (this.size() === 1) return this.heap.pop();

        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown(0);

        return max;
    }

    // Get the current size of the heap
    size() {
        return this.heap.length;
    }

    // Helper function to maintain max heap property on insertion
    bubbleUp(index) {
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[parentIndex] >= this.heap[index]) break;

            [this.heap[parentIndex], this.heap[index]] = [this.heap[index], this.heap[parentIndex]];
            index = parentIndex;
        }
    }

    // Helper function to maintain max heap property on extraction
    bubbleDown(index) {
        const length = this.heap.length;
        let largest = index;

        while (true) {
            let left = 2 * index + 1;
            let right = 2 * index + 2;

            if (left < length && this.heap[left] > this.heap[largest]) {
                largest = left;
            }
            if (right < length && this.heap[right] > this.heap[largest]) {
                largest = right;
            }
            if (largest === index) break;

            [this.heap[index], this.heap[largest]] = [this.heap[largest], this.heap[index]];
            index = largest;
        }
    }
}

function maxKelements(nums, k) {
    const maxHeap = new MaxHeap();

    // Insert all elements into the max heap
    for (const num of nums) {
        maxHeap.insert(num);
    }

    let score = 0;

    // Perform k operations
    for (let i = 0; i < k; i++) {
        const maxVal = maxHeap.extractMax();     // Extract the max element
        score += maxVal;                         // Increase score by the max element
        const newVal = Math.ceil(maxVal / 3);    // Replace with ceil(maxVal / 3)
        maxHeap.insert(newVal);                  // Insert the new value back into the heap
    }

    return score;
}

// Example usage:
console.log(maxKelements([10, 10, 10, 10, 10], 5)); // Output: 50
console.log(maxKelements([1, 10, 3, 3, 3], 3));     // Output: 17

```