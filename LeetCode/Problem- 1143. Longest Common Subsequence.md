Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Dynamic Programming]]

In this [[DSA]] problem, given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

## Approach-
This problem can be solved using ==form 3== of DP.
We'll set two pointers in the beginning of string A and B respectively.
If they match, we return 1 + dp(moving both ahead by 1).
But if they don't match, we take the max of 3 choices
1. moving only i ahead by 1
2. moving only j ahead by 1
3. moving both ahead by 1

(I've also added the code to print the solution by retracing our steps)

### Code 
```cpp
class Solution {
public:
    int rec(string &A, string &B, vector<vector<int>> &dp, int i, int j) {
        if(i >= A.size() || j >= B.size()) return 0;

        if(dp[i][j] != -1) return dp[i][j];

        int ans = 0;

        if(A[i] == B[j]) ans =  1 + rec(A, B, dp, i+1, j+1);
        else {
            ans = max({
                rec(A, B, dp, i+1, j+1),
                rec(A, B, dp, i+1, j),
                rec(A, B, dp, i, j+1)
            });
        } 


        return dp[i][j] = ans;
    }

    int longestCommonSubsequence(string text1, string text2) {
        int sA = text1.size();
        int sB = text2.size();
        vector<vector<int>> dp(sA + 1, vector<int> (sB + 1, -1));

        return rec(text1, text2, dp, 0, 0);
    }
};
```
