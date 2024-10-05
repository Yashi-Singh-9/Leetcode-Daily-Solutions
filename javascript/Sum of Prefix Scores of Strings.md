# Sum of Prefix Scores of Strings

## Problem Statement

You are given an array `words` of size `n` consisting of non-empty strings.

We define the score of a string `term` as the number of strings `words[i]` such that `term` is a **prefix** of `words[i]`.

- For example, if `words = ["a", "ab", "abc", "cab"]`, then the score of `"ab"` is `2`, since `"ab"` is a prefix of both `"ab"` and `"abc"`.

*Return an array `answer` of size `n` where `answer[i]` is the **sum** of scores of every **non-empty** prefix of `words[i]`.*

**Note** that a string is considered as a prefix of itself.

**Example 1:**

**Input:** words = ["abc","ab","bc","b"]
**Output:** [5,4,3,2]
**Explanation:** The answer for each string is the following:
- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is answer[0] = 2 + 2 + 1 = 5.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is answer[1] = 2 + 2 = 4.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is answer[2] = 2 + 1 = 3.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is answer[3] = 2.

**Example 2:**

**Input:** words = ["abcd"]
**Output:** [4]
**Explanation:**
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is answer[0] = 1 + 1 + 1 + 1 = 4.
 

**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` consists of lowercase English letters.

## Solution

<!-- Solution in javascript -->

class TrieNode {
    constructor() {
        this.children = {};
        this.count = 0; // Counts how many words pass through this node
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    // Insert a word into the Trie and update counts for prefixes
    insert(word) {
        let node = this.root;
        for (const char of word) {
            if (!node.children[char]) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
            node.count++; // Increment the count for this prefix
        }
    }

    // Calculate the total prefix scores for the given word
    sumPrefixScores(word) {
        let node = this.root;
        let totalScore = 0;
        for (const char of word) {
            if (node.children[char]) {
                node = node.children[char];
                totalScore += node.count; // Add the count of words with this prefix
            } else {
                break; // No further prefixes exist
            }
        }
        return totalScore;
    }
}

function sumPrefixScores(words) {
    const trie = new Trie();

    // Insert all words into the Trie
    for (const word of words) {
        trie.insert(word);
    }

    // Calculate the total prefix scores for each word
    const result = [];
    for (const word of words) {
        let scoreSum = 0;
        let node = trie.root;
        for (const char of word) {
            if (node.children[char]) {
                node = node.children[char];
                scoreSum += node.count; // Sum the score for this prefix
            } else {
                break; // No further prefixes exist
            }
        }
        result.push(scoreSum); // Store the total score for this word
    }

    return result;
}

// Example Usage
const words = ["abc", "ab", "bc", "b"];
console.log(sumPrefixScores(words)); // Output: [5, 4, 3, 2]
