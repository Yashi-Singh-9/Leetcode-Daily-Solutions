# Divide Intervals Into Minimum Number of Groups

You are given a 2D integer array `intervals` where intervals[i] = [left<sub>i</sub>, right<sub>i</sub>] represents the inclusive interval [left<sub>i<sub>, right<sub>i</sub>].

You have to divide the intervals into one or more `groups` such that each interval is in `exactly` one group, and no two intervals that are in the same group `intersect` each other.

Return *the **minimum** number of groups you need to make.*

Two intervals **intersect** if there is at least one common number between them. For example, the intervals `[1, 5]` and `[5, 8]` intersect.

 

**Example 1:**
```
Input: intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
Output: 3
Explanation: We can divide the intervals into the following groups:
- Group 1: [1, 5], [6, 8].
- Group 2: [2, 3], [5, 10].
- Group 3: [1, 10].
It can be proven that it is not possible to divide the intervals into fewer than 3 groups.
```

**Example 2:**
```
Input: intervals = [[1,3],[5,6],[8,10],[11,13]]
Output: 1
Explanation: None of the intervals overlap, so we can put all of them in one group.
 ```

**Constraints:**

- 1 <= intervals.length <= 10<sup>5</sup>
- `intervals[i].length == 2`
- 1 <= left<sub>i</sub> <= right<sub>i</sub> <= 10<sup>6</sup>

### Answer:

```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    // Insert a new value into the heap
    insert(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    
    // Remove and return the smallest value from the heap
    extractMin() {
        if (this.heap.length === 1) return this.heap.pop();
        const min = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown();
        return min;
    }
    
    // Return the smallest value from the heap without removing it
    peek() {
        return this.heap[0];
    }
    
    // Helper function to maintain the min-heap property after insertion
    bubbleUp() {
        let index = this.heap.length - 1;
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] >= this.heap[parentIndex]) break;
            [this.heap[index], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[index]];
            index = parentIndex;
        }
    }
    
    // Helper function to maintain the min-heap property after extraction
    bubbleDown() {
        let index = 0;
        const length = this.heap.length;
        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let smallest = index;
            
            if (leftChildIndex < length && this.heap[leftChildIndex] < this.heap[smallest]) {
                smallest = leftChildIndex;
            }
            if (rightChildIndex < length && this.heap[rightChildIndex] < this.heap[smallest]) {
                smallest = rightChildIndex;
            }
            if (smallest === index) break;
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            index = smallest;
        }
    }
    
    // Check if the heap is empty
    isEmpty() {
        return this.heap.length === 0;
    }
}

var minGroups = function(intervals) {
    // Sort intervals by their start time
    intervals.sort((a, b) => a[0] - b[0]);

    // Initialize a MinHeap to track end times of the groups
    let heap = new MinHeap();
    
    for (let interval of intervals) {
        let [start, end] = interval;
        
        // If the heap is not empty and the smallest end time is less than the start of the current interval
        if (!heap.isEmpty() && heap.peek() < start) {
            // Reuse the group by replacing the end time with the current interval's end
            heap.extractMin();
        }
        
        // Add the current interval's end time to the heap
        heap.insert(end);
    }

    // The size of the heap is the number of groups needed
    return heap.heap.length;
};
```