# Maximum Width Ramp

A **ramp** in an integer array `nums` is a pair `(i, j)` for which `i < j` and `nums[i] <= nums[j]`. The **width** of such a ramp is `j - i`.

Given an integer array `nums`, return *the maximum width of a **ramp** in `nums`*. If there is no **ramp** in `nums`, return `0`.
 
**Example 1:**

```
Input: nums = [6,0,8,2,1,5]
Output: 4
Explanation: The maximum width ramp is achieved at (i, j) = (1, 5): nums[1] = 0 and nums[5] = 5.
```

**Example 2:**
```
Input: nums = [9,8,1,0,1,9,4,0,4,1]
Output: 7
Explanation: The maximum width ramp is achieved at (i, j) = (2, 9): nums[2] = 1 and nums[9] = 1.
 ```

**Constraints:**

- 2 <= nums.length <= 5 * 10<sup>4</sup>
- 0 <= nums[i] <= 5 * 10<sup>4</sup>

### Answer:

```javascript
function maxWidthRamp(nums) {
    // Create an array of indices and sort them by values in nums
    let indices = Array.from(Array(nums.length).keys());
    indices.sort((a, b) => nums[a] - nums[b] || a - b);

    let maxWidth = 0;
    let minIndex = nums.length;

    // Calculate the maximum ramp width
    for (let index of indices) {
        maxWidth = Math.max(maxWidth, index - minIndex);
        minIndex = Math.min(minIndex, index);
    }

    return maxWidth;
}

// Example usage:
console.log(maxWidthRamp([6, 0, 8, 2, 1, 5])); // Output: 4
console.log(maxWidthRamp([9, 8, 1, 0, 1, 9, 4, 0, 4, 1])); // Output: 7

```