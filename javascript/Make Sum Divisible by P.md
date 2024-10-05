# Make Sum Divisible by P

### Description

Given an array of positive integers `nums`, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by `p`. It is not allowed to remove the whole array.

*Return the length of the smallest subarray that you need to remove, or `-1` if it's impossible.*

A **subarray** is defined as a contiguous block of elements in the array.

 

**Example 1:**
```
Input: nums = [3,1,4,2], p = 6
Output: 1
Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.
```

**Example 2:**
```
Input: nums = [6,3,5,2], p = 9
Output: 2
Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.
```

**Example 3:**
```
Input: nums = [1,2,3], p = 3
Output: 0
Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
 ```

**Constraints:**

- 1 <= nums.length <= 10<sup>5</sup>
- 1 <= nums[i] <= 10<sup>9</sup>
- 1 <= p <= 10<sup>9</sup>

### Solution

```javascript
function minSubarray(nums, p) {
    const totalSum = nums.reduce((sum, num) => sum + num, 0); // Calculate total sum of the array
    const remainder = totalSum % p; // Calculate remainder when divided by p
    
    // If remainder is 0, the total sum is already divisible by p, no need to remove any subarray
    if (remainder === 0) {
        return 0;
    }

    // Use a map to track the prefix sums and their corresponding indices
    const prefixMap = new Map();
    prefixMap.set(0, -1); // Initialize the map with 0 at index -1 to handle edge cases

    let prefixSum = 0;
    let minLength = nums.length;

    // Iterate through the array and calculate prefix sums
    for (let i = 0; i < nums.length; i++) {
        prefixSum = (prefixSum + nums[i]) % p; // Calculate the prefix sum modulo p
        const target = (prefixSum - remainder + p) % p; // Calculate the target we want to find in the map

        // Check if the target exists in the map
        if (prefixMap.has(target)) {
            minLength = Math.min(minLength, i - prefixMap.get(target)); // Update the minimum length
        }

        // Update the map with the current prefix sum and index
        prefixMap.set(prefixSum, i);
    }

    // If no valid subarray was found, return -1, otherwise return the minimum length
    return minLength === nums.length ? -1 : minLength;
}
```
