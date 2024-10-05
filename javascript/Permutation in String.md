# Permutation in String

### Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a [permutation](A permutation is a rearrangement of all the characters of a string.) of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

 
***
Example 1:

Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
Example 2:

Input: s1 = "ab", s2 = "eidboaoo"
Output: false
*** 

**Constraints:**

- 1 <= s1.length, s2.length <= 10^4
- `s1` and `s2` consist of lowercase English letters.

### Solution

```javascript
function checkInclusion(s1, s2) {
    if (s1.length > s2.length) return false;

    // Initialize frequency arrays for both strings
    const s1Freq = new Array(26).fill(0);
    const s2Freq = new Array(26).fill(0);
    
    // Fill frequency array for s1 and the first window of s2
    for (let i = 0; i < s1.length; i++) {
        s1Freq[s1.charCodeAt(i) - 97]++;
        s2Freq[s2.charCodeAt(i) - 97]++;
    }

    // Compare the initial window
    if (arraysMatch(s1Freq, s2Freq)) return true;

    // Now slide the window across s2
    for (let i = s1.length; i < s2.length; i++) {
        s2Freq[s2.charCodeAt(i) - 97]++;           // Include next character in window
        s2Freq[s2.charCodeAt(i - s1.length) - 97]--; // Remove the character going out of the window

        if (arraysMatch(s1Freq, s2Freq)) return true;
    }

    return false;
}

// Helper function to check if two frequency arrays match
function arraysMatch(arr1, arr2) {
    for (let i = 0; i < 26; i++) {
        if (arr1[i] !== arr2[i]) return false;
    }
    return true;
}

// Example usage:
console.log(checkInclusion("ab", "eidbaooo")); // Output: true
console.log(checkInclusion("ab", "eidboaoo")); // Output: false
```