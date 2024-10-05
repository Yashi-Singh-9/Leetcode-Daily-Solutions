# Divide Players Into Teams of Equal Skill

### Description

You are given a positive integer array `skill` of even length `n` where `skill[i]` denotes the skill of the `i^th` player. Divide the players into `n / 2` teams of size `2` such that the total skill of each team is **equal**.

The **chemistry** of a team is equal to the **product** of the skills of the players on that team.

*Return the sum of the **chemistry** of all the teams, or return `-1` if there is no way to divide the players into teams such that the total skill of each team is equal.*
 
```
Example 1:

Input: skill = [3,2,5,1,3,4]
Output: 22
Explanation: 
Divide the players into the following teams: (1, 5), (2, 4), (3, 3), where each team has a total skill of 6.
The sum of the chemistry of all the teams is: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.

Example 2:

Input: skill = [3,4]
Output: 12
Explanation: 
The two players form a team with a total skill of 7.
The chemistry of the team is 3 * 4 = 12.

Example 3:

Input: skill = [1,1,2,3]
Output: -1
Explanation: 
There is no way to divide the players into teams such that the total skill of each team is equal.
``` 

**Constraints:**

- `2 <= skill.length <= 10^5`
- `skill.length` is even.
- `1 <= skill[i] <= 1000`

### Solution

```javascript
function dividePlayers(skill) {
    skill.sort((a, b) => a - b);  // Sort the skill array in ascending order
    let n = skill.length;
    let targetSum = skill[0] + skill[n - 1];  // The target sum for each pair
    
    let totalChemistry = 0;
    for (let i = 0; i < n / 2; i++) {
        // Check if the sum of the current pair equals the target sum
        if (skill[i] + skill[n - 1 - i] !== targetSum) {
            return -1;  // If not, return -1 as no valid team formation is possible
        }
        // Add the chemistry (product of the current pair) to the total chemistry
        totalChemistry += skill[i] * skill[n - 1 - i];
    }
    
    return totalChemistry;
}
```