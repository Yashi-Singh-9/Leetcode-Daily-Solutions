# Maximum Swap

You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return *the maximum valued number you can get*.

**Example 1:**
```
Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```

**Example 2:**
```
Input: num = 9973
Output: 9973
Explanation: No swap.
```

**Constraints:**

- 0 <= num <= 10<sup>8</sup>

### Answer:

```javascript
function maximumSwap(num) {
    // Convert the number to an array of digits
    const digits = num.toString().split('').map(Number);
    
    // Create an array to store the last position of each digit from 0 to 9
    const lastIndex = Array(10).fill(-1);
    for (let i = 0; i < digits.length; i++) {
        lastIndex[digits[i]] = i;
    }

    // Try to find the first position where a larger digit can be swapped
    for (let i = 0; i < digits.length; i++) {
        // Check from 9 down to the current digit to see if a larger digit exists later
        for (let d = 9; d > digits[i]; d--) {
            if (lastIndex[d] > i) {
                // Swap the digits at positions i and lastIndex[d]
                [digits[i], digits[lastIndex[d]]] = [digits[lastIndex[d]], digits[i]];
                
                // Return the new number after the swap
                return parseInt(digits.join(''), 10);
            }
        }
    }

    // If no swap was made, return the original number
    return num;
}

// Example usage:
console.log(maximumSwap(2736)); // Output: 7236
console.log(maximumSwap(9973)); // Output: 9973
```