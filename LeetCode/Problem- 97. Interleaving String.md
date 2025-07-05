Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Dynamic Programming]], [[Backtracking]]

In this [[DSA]] problem, given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t`are divided into `n` and `m` substrings respectively, such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
**Output:** true
**Explanation:** One way to obtain s3 is:
Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
Since s3 can be obtained by interleaving s1 and s2, we return true.

**Example 2:**

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
**Output:** false
**Explanation:** Notice how it is impossible to interleave s2 with any other string to obtain s3.

**Example 3:**

**Input:** s1 = "", s2 = "", s3 = ""
**Output:** true

**Constraints:**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

## Approach 
We can use ==form 4== of dp for this problem where we will place pointers at the beginning of both strings and see if the ith index of s3 matches with either of the strings' characters where their pointers are pointing. If they do, we move the particular pointer ahead of that string whose character matches with s3. And in doing so, if both pointers reach the end of s1 and s2, it means all characters have been successfully matched.

### Code 
```cpp
class Solution {
public:
    int isPossible(int i, int j, string s1, string s2, string s3, vector<vector<int>> &dp) {
        // base case 
        if(i == s1.size() && j == s2.size()) return 1;

        // dp check
        if(dp[i][j] != -1) return dp[i][j];

        // transition
        int ans = 0;
        if(i < s1.size() && s3[i+j] == s1[i]) ans |= isPossible(i+1, j, s1, s2, s3, dp);
        if(j < s2.size() && s3[i+j] == s2[j]) ans |= isPossible(i, j+1, s1, s2, s3, dp);

        // save and return
        return dp[i][j] = ans;
    }

    bool isInterleave(string s1, string s2, string s3) {
        vector<vector<int>> dp(s1.size() + 1, vector<int> (s2.size() + 1, -1));

        if(s1.size() + s2.size() != s3.size()) return false;

        return (isPossible(0, 0, s1, s2, s3, dp));
    }
};
```

