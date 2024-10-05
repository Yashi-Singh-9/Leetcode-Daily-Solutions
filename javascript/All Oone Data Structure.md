# All O`one Data Structure

### Description

Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the `AllOne` class:

- `AllOne()` Initializes the object of the data structure.
- `inc(String key)` Increments the count of the string `key` by `1`. If `key` does not exist in the data structure, insert it with count `1`.
- `dec(String key)` Decrements the count of the string `key` by `1`. If the count of `key` is `0` after the decrement, remove it from the data structure. It is guaranteed that `key` exists in the data structure before the decrement.
- `getMaxKey()` Returns one of the keys with the maximal count. If no element exists, return an empty string `""`.
- `getMinKey()` Returns one of the keys with the minimum count. If no element exists, return an empty string `""`.

**Note** that each function must run in `O(1)` average time complexity.
```
Example 1:

Input:

["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]

Output:
[null, null, null, "hello", "hello", null, "hello", "leet"]

**Explanation**
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "hello"
allOne.inc("leet");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "leet"
 ```

**Constraints:**

- `1 <= key.length <= 10`
- `key` consists of lowercase English letters.
- It is guaranteed that for each call to `dec`, `key` is existing in the data structure.
- At most 5 * 10<sup>4</sup> calls will be made to `inc`, `dec`, `getMaxKey`, and `getMinKey`.

### Solution

```javascript
class Node {
    constructor(count) {
        this.count = count; // Count of keys at this node
        this.keys = new Set(); // Set to store keys with the same count
        this.prev = null; // Pointer to the previous node
        this.next = null; // Pointer to the next node
    }
}

class AllOne {
    constructor() {
        this.countMap = new Map(); // Maps keys to their counts
        this.nodeMap = new Map(); // Maps counts to their corresponding nodes in the linked list
        this.head = new Node(-Infinity); // Dummy head
        this.tail = new Node(Infinity); // Dummy tail
        this.head.next = this.tail; // Connect head to tail
        this.tail.prev = this.head; // Connect tail to head
    }

    // Helper function to add a new node after a given previous node
    _addNodeAfter(newNode, prevNode) {
        newNode.prev = prevNode;
        newNode.next = prevNode.next;
        prevNode.next.prev = newNode;
        prevNode.next = newNode;
    }

    // Helper function to remove a node from the linked list
    _removeNode(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        this.nodeMap.delete(node.count);
    }

    inc(key) {
        // Increment the count of the key
        const currentCount = this.countMap.get(key) || 0;
        const newCount = currentCount + 1;

        // Update the countMap with the new count
        this.countMap.set(key, newCount);

        // Remove the key from the current count node
        if (currentCount > 0) {
            const currentNode = this.nodeMap.get(currentCount);
            currentNode.keys.delete(key);
            if (currentNode.keys.size === 0) {
                this._removeNode(currentNode); // Remove the node if empty
            }
        }

        // Create or get the node for the new count
        let newNode = this.nodeMap.get(newCount);
        if (!newNode) {
            newNode = new Node(newCount);
            this.nodeMap.set(newCount, newNode);

            // Insert the new node in the linked list
            let prevNode = this.head; // Start from the head
            while (prevNode.next.count < newCount) { // Find the right position to insert
                prevNode = prevNode.next;
            }
            this._addNodeAfter(newNode, prevNode);
        }

        // Add the key to the new count node
        newNode.keys.add(key);
    }

    dec(key) {
        // Decrement the count of the key
        const currentCount = this.countMap.get(key);
        const newCount = currentCount - 1;

        // Remove the key from the countMap
        if (newCount === 0) {
            this.countMap.delete(key); // Remove the key if count is 0
        } else {
            this.countMap.set(key, newCount); // Update the countMap
        }

        // Remove the key from the current count node
        const currentNode = this.nodeMap.get(currentCount);
        currentNode.keys.delete(key);
        if (currentNode.keys.size === 0) {
            this._removeNode(currentNode); // Remove the node if empty
        }

        // Create or get the node for the new count
        if (newCount > 0) {
            let newNode = this.nodeMap.get(newCount);
            if (!newNode) {
                newNode = new Node(newCount);
                this.nodeMap.set(newCount, newNode);

                // Insert the new node in the linked list
                let prevNode = this.head; // Start from the head
                while (prevNode.next.count < newCount) { // Find the right position to insert
                    prevNode = prevNode.next;
                }
                this._addNodeAfter(newNode, prevNode);
            }

            // Add the key to the new count node
            newNode.keys.add(key);
        }
    }

    getMaxKey() {
        // Return one key with the maximum count
        if (this.tail.prev === this.head) {
            return ""; // No elements
        }
        return this.tail.prev.keys.values().next().value; // Get an arbitrary key from the max count
    }

    getMinKey() {
        // Return one key with the minimum count
        if (this.head.next === this.tail) {
            return ""; // No elements
        }
        return this.head.next.keys.values().next().value; // Get an arbitrary key from the min count
    }
}
```
