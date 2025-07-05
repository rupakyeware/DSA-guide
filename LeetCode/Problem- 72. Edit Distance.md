Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]], [[Dynamic Programming]]

In this [[DSA]] problem, given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

## Approach 
Solved this using ==form 3==. It was pretty simple as I've solved a harder variation of this problem -[[Problem- Print minimum diff utility string]]. It's basically the same problem just without the printing part.

### Code 

``` cpp
class Solution {
public:
    int m, n;
    string A, B;
    int INF = 1001001;

    int rec(int i, int j, vector<vector<int>> &dp) {
        if(i == m && j == n) return 0;
        if(i == m) return n - j;
        if(j == n) return m - i;

        // dp check
        if(dp[i][j] != -1) return dp[i][j];

        // transition
        int ans = 0;

        if(A[i] == B[j]) {
            ans = rec(i+1, j+1, dp);
        }
        else {
            ans = 1 + min({
                rec(i, j+1, dp), // inserting a character
                rec(i+1, j, dp), // deleting a character
                rec(i+1, j+1, dp) // replacing a character
            });
        }

        // save and return
        return dp[i][j] = ans;
    }

    int minDistance(string word1, string word2) {
        A = word1; B = word2;
        m = word1.size(); n = word2.size();

        vector<vector<int>> dp(m+1, vector<int> (n+1, -1));

        return rec(0, 0, dp);
    }
};
```
