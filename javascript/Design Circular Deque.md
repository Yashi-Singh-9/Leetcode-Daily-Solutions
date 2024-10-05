# Design Circular Deque

### Problem Statement 

Design your implementation of the circular double-ended queue (deque).

Implement the `MyCircularDeque` class:

- `MyCircularDeque(int k)` Initializes the deque with a maximum size of `k`.
- `boolean insertFront()` Adds an item at the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean insertLast()` Adds an item at the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean deleteFront()` Deletes an item from the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `boolean deleteLast()` Deletes an item from the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
- `int getFront()` Returns the front item from the Deque. Returns `-1` if the deque is empty.
- `int getRear()` Returns the last item from Deque. Returns `-1` if the deque is empty.
- `boolean isEmpty()` Returns `true` if the deque is empty, or `false` otherwise.
- `boolean isFull()` Returns `true` if the deque is full, or `false` otherwise.
 
```
Example 1:

Input:

["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]

Output:

[null, true, true, true, false, 2, true, true, true, 4]

Explanation:

MyCircularDeque myCircularDeque = new MyCircularDeque(3);
myCircularDeque.insertLast(1);  // return True
myCircularDeque.insertLast(2);  // return True
myCircularDeque.insertFront(3); // return True
myCircularDeque.insertFront(4); // return False, the queue is full.
myCircularDeque.getRear();      // return 2
myCircularDeque.isFull();       // return True
myCircularDeque.deleteLast();   // return True
myCircularDeque.insertFront(4); // return True
myCircularDeque.getFront();     // return 4
``` 

**Constraints:**

- `1 <= k <= 1000`
- `0 <= value <= 1000`
- At most `2000` calls will be made to `insertFront`, `insertLast`, `deleteFront`, `deleteLast`, `getFront`, `getRear`, `isEmpty`, `isFull`.

### Solution 

```javascript
class MyCircularDeque {
    constructor(k) {
        this.k = k;
        this.deque = Array(k).fill(null);  // Initialize array to hold elements
        this.front = 0;  // Points to the front
        this.rear = 0;   // Points to the rear
        this.size = 0;   // Track the current size of the deque
    }

    insertFront(value) {
        if (this.isFull()) {
            return false;  // Deque is full, cannot insert
        }
        this.front = (this.front - 1 + this.k) % this.k;  // Circular decrement front pointer
        this.deque[this.front] = value;  // Add value at the front
        this.size++;  // Increase size
        return true;
    }

    insertLast(value) {
        if (this.isFull()) {
            return false;  // Deque is full, cannot insert
        }
        this.deque[this.rear] = value;  // Add value at the rear
        this.rear = (this.rear + 1) % this.k;  // Circular increment rear pointer
        this.size++;  // Increase size
        return true;
    }

    deleteFront() {
        if (this.isEmpty()) {
            return false;  // Deque is empty, cannot delete
        }
        this.front = (this.front + 1) % this.k;  // Circular increment front pointer
        this.size--;  // Decrease size
        return true;
    }

    deleteLast() {
        if (this.isEmpty()) {
            return false;  // Deque is empty, cannot delete
        }
        this.rear = (this.rear - 1 + this.k) % this.k;  // Circular decrement rear pointer
        this.size--;  // Decrease size
        return true;
    }

    getFront() {
        if (this.isEmpty()) {
            return -1;  // Deque is empty
        }
        return this.deque[this.front];  // Return front element
    }

    getRear() {
        if (this.isEmpty()) {
            return -1;  // Deque is empty
        }
        return this.deque[(this.rear - 1 + this.k) % this.k];  // Return rear element
    }

    isEmpty() {
        return this.size === 0;  // Check if deque is empty
    }

    isFull() {
        return this.size === this.k;  // Check if deque is full
    }
}
```
