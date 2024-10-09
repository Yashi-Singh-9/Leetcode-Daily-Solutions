# Minimum Add to Make Parentheses Valid

A parentheses string is valid if and only if:

- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.
  
You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(()))"` or a closing parenthesis to be `"())))"`.

*Return the minimum number of moves required to make `s` valid.*

**Example 1:**
```
Input: s = "())"
Output: 1
```

**Example 2:**
```
Input: s = "((("
Output: 3
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` is either `'('` or `')'`.

### Answer: 
```javascript
function minAddToMakeValid(s) {
    let openCount = 0; // To track unmatched opening parentheses '('
    let moves = 0;     // To track the number of moves required

    for (let char of s) {
        if (char === '(') {
            openCount++; // Increment for an unmatched '('
        } else {
            if (openCount > 0) {
                openCount--; // A matched pair is found, decrement openCount
            } else {
                moves++; // An unmatched ')' found, increment moves
            }
        }
    }

    // Add unmatched '(' to moves as they require a closing ')'
    moves += openCount;

    return moves;
}

// Example usage:
console.log(minAddToMakeValid("())"));  // Output: 1
console.log(minAddToMakeValid("((("));  // Output: 3
```
