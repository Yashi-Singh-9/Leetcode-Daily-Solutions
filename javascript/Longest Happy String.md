# Longest Happy String

A string `s` is called **happy** if it satisfies the following conditions:

- `s` only contains the letters `'a'`, `'b'`, and `'c'`.
- `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
- `s` contains **at most** `a` occurrences of the letter `'a'`.
- `s` contains **at most** `b` occurrences of the letter `'b'`.
- `s` contains **at most** `c` occurrences of the letter `'c'`.

Given three integers `a`, `b`, and `c`, return the   **longest possible happy** string. If there are multiple longest happy strings, return any of them. If there is no such string, return the empty string `""`.

A substring is a contiguous sequence of characters within a string.

**Example 1:**
```
Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.
```

**Example 2:**
```
Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.
 ```

**Constraints:**

- `0 <= a, b, c <= 100`
- `a + b + c > 0`

### Answer: 
```javascript
function longestDiverseString(a, b, c) {
    // Array to store counts of each character and the character itself
    let counts = [
        [a, 'a'],
        [b, 'b'],
        [c, 'c']
    ];
    
    // Result string
    let result = '';

    while (true) {
        // Sort counts by frequency in descending order
        counts.sort((x, y) => y[0] - x[0]);

        let hasAdded = false;

        // Iterate through the counts array to try and add characters
        for (let i = 0; i < 3; i++) {
            let [count, char] = counts[i];
            
            // Check if the current character can be added without breaking the "happy" string rule
            if (count > 0 && (result.length < 2 || result.slice(-2) !== char + char)) {
                // Append character to the result and decrement the count
                result += char;
                counts[i][0]--;
                hasAdded = true;
                break;
            }
        }
        
        // If no character could be added, break the loop
        if (!hasAdded) {
            break;
        }
    }
    
    return result;
}

// Example usage:
console.log(longestDiverseString(1, 1, 7)); // Output: "ccaccbcc" or similar
console.log(longestDiverseString(7, 1, 0)); // Output: "aabaa"
```