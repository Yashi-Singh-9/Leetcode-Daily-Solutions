# Rank Transform of an Array

### Description 

Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

- Rank is an integer starting from 1.
- The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
- Rank should be as small as possible.
 

**Example 1:**
```
Input: arr = [40,10,20,30]
Output: [4,1,2,3]
Explanation: 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.
```

**Example 2:**
```
Input: arr = [100,100,100]
Output: [1,1,1]
Explanation: Same elements share the same rank.
```

**Example 3:**
```
Input: arr = [37,12,28,9,100,56,80,5,12]
Output: [5,3,4,2,8,6,7,1,3]
```

**Constraints:**

- ``0 <= arr.length <= 10^5``
- ``-10^9 <= arr[i] <= 10^9``

### Solution

```javascript
function arrayRankTransform(arr) {
    // Step 1: Create a sorted array of unique elements
    const sortedUnique = [...new Set(arr)].sort((a, b) => a - b);

    // Step 2: Create a rank map, assigning rank to each element
    const rankMap = {};
    sortedUnique.forEach((val, idx) => {
        rankMap[val] = idx + 1; // Rank starts from 1
    });

    // Step 3: Replace elements in the original array with their ranks
    return arr.map(val => rankMap[val]);
}

// Example 1:
console.log(arrayRankTransform([40, 10, 20, 30]));  // Output: [4, 1, 2, 3]

// Example 2:
console.log(arrayRankTransform([100, 100, 100]));  // Output: [1, 1, 1]

// Example 3:
console.log(arrayRankTransform([37, 12, 28, 9, 100, 56, 80, 5, 12]));  // Output: [5, 3, 4, 2, 8, 6, 7, 1, 3]
```