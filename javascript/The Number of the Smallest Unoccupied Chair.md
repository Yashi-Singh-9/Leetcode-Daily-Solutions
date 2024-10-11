# The Number of the Smallest Unoccupied Chair

There is a party where `n` friends numbered from `0` to `n - 1` are attending. There is an **infinite** number of chairs in this party that are numbered from `0` to `infinity`. When a friend arrives at the party, they sit on the unoccupied chair with the **smallest number**.

- For example, if chairs `0`, `1`, and `5` are occupied when a friend comes, they will sit on chair number `2`.

When a friend leaves the party, their chair becomes unoccupied at the moment they leave. If another friend arrives at that same moment, they can sit in that chair.

You are given a **0-indexed** 2D integer array `times` where times[i] = [arrival<sub>i</sub>, leaving<sub>i</sub>], indicating the arrival and leaving times of the i<sup>th</sup> friend respectively, and an integer `targetFriend`. All arrival times are **distinct**.

Return *the **chair number** that the friend numbered `targetFriend` will sit on*.

**Example 1:**
```
Input: times = [[1,4],[2,3],[4,6]], targetFriend = 1
Output: 1
Explanation: 
- Friend 0 arrives at time 1 and sits on chair 0.
- Friend 1 arrives at time 2 and sits on chair 1.
- Friend 1 leaves at time 3 and chair 1 becomes empty.
- Friend 0 leaves at time 4 and chair 0 becomes empty.
- Friend 2 arrives at time 4 and sits on chair 0.
Since friend 1 sat on chair 1, we return 1.
```

**Example 2:**
```
Input: times = [[3,10],[1,5],[2,6]], targetFriend = 0
Output: 2
Explanation: 
- Friend 1 arrives at time 1 and sits on chair 0.
- Friend 2 arrives at time 2 and sits on chair 1.
- Friend 0 arrives at time 3 and sits on chair 2.
- Friend 1 leaves at time 5 and chair 0 becomes empty.
- Friend 2 leaves at time 6 and chair 1 becomes empty.
- Friend 0 leaves at time 10 and chair 2 becomes empty.
Since friend 0 sat on chair 2, we return 2.
```

**Constraints:**

- `n == times.length`
- 2 <= n <= 10<sup>4</sup>
- `times[i].length == 2`
- 1 <= arrival<sub>i</sub> < leaving<sub>i</sub> <= 10<sup>5</sup>
- `0 <= targetFriend <= n - 1`
- Each arrival<sub>i</sub> time is distinct.

### Answer:

```javascript
function smallestChair(times, targetFriend) {
    const n = times.length;
    const ts = new Array(n);
    
    // Priority Queue for available chairs
    const availableChairs = new MinHeap();
    
    // Priority Queue for occupied chairs with leave time
    const busyChairs = new MinHeap((a, b) => a[0] - b[0]);

    // Initialize all friends and available chairs
    for (let i = 0; i < n; i++) {
        ts[i] = [times[i][0], times[i][1], i];
        availableChairs.push(i);
    }
    
    // Sort friends by arrival time
    ts.sort((a, b) => a[0] - b[0]);

    // Process each friend's arrival and departure
    for (const [arrival, departure, friendIndex] of ts) {
        // Free up chairs that are available by current arrival time
        while (busyChairs.size() > 0 && busyChairs.peek()[0] <= arrival) {
            availableChairs.push(busyChairs.pop()[1]);
        }

        // Assign the smallest available chair
        const chair = availableChairs.pop();

        // If this is the target friend, return the chair number
        if (friendIndex === targetFriend) {
            return chair;
        }

        // Mark this chair as busy until the friend's departure
        busyChairs.push([departure, chair]);
    }

    return -1; // Fallback case
}

// MinHeap class with customizable comparator
class MinHeap {
    constructor(compareFn = (a, b) => a - b) {
        this.heap = [];
        this.compareFn = compareFn;
    }

    push(val) {
        this.heap.push(val);
        this._bubbleUp();
    }

    pop() {
        if (this.heap.length === 1) return this.heap.pop();
        const min = this.heap[0];
        this.heap[0] = this.heap.pop();
        this._sinkDown();
        return min;
    }

    peek() {
        return this.heap[0];
    }

    size() {
        return this.heap.length;
    }

    _bubbleUp() {
        let idx = this.heap.length - 1;
        const element = this.heap[idx];
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2);
            let parent = this.heap[parentIdx];
            if (this.compareFn(element, parent) >= 0) break;
            this.heap[parentIdx] = element;
            this.heap[idx] = parent;
            idx = parentIdx;
        }
    }

    _sinkDown() {
        let idx = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        while (true) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let swap = null;
            if (leftChildIdx < length) {
                let leftChild = this.heap[leftChildIdx];
                if (this.compareFn(leftChild, element) < 0) swap = leftChildIdx;
            }
            if (rightChildIdx < length) {
                let rightChild = this.heap[rightChildIdx];
                if (this.compareFn(rightChild, element) < 0 && this.compareFn(rightChild, this.heap[leftChildIdx]) < 0) swap = rightChildIdx;
            }
            if (swap === null) break;
            this.heap[idx] = this.heap[swap];
            this.heap[swap] = element;
            idx = swap;
        }
    }
}

// Example Usage:
console.log(smallestChair([[1, 4], [2, 3], [4, 6]], 1)); // Output: 1
console.log(smallestChair([[3, 10], [1, 5], [2, 6]], 0)); // Output: 2
```
