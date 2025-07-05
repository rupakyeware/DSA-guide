Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you have intercepted a secret message encoded as a string of numbers. The message is **decoded** via the following mapping:

`"1" -> 'A'   "2" -> 'B'   ...   "25" -> 'Y'   "26" -> 'Z'`

However, while decoding the message, you realize that there are many different ways you can decode the message because some codes are contained in other codes (`"2"` and `"5"` vs `"25"`).

For example, `"11106"` can be decoded into:

- `"AAJF"` with the grouping `(1, 1, 10, 6)`
- `"KJF"` with the grouping `(11, 10, 6)`
- The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).

Note: there may be strings that are impossible to decode.  
  
Given a string s containing only digits, return the **number of ways** to **decode** it. If the entire string cannot be decoded in any valid way, return `0`.

The test cases are generated so that the answer fits in a **32-bit** integer.

**Example 1:**

**Input:** s = "12"

**Output:** 2

**Explanation:**

"12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2:**

**Input:** s = "226"

**Output:** 3

**Explanation:**

"226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

**Example 3:**

**Input:** s = "06"

**Output:** 0

**Explanation:**

"06" cannot be mapped to "F" because of the leading zero ("6" is different from "06"). In this case, the string is not a valid encoding, so return 0.

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).

## Approach 
In this problem, we have to kind of approach it like [[Problem- 198. House Robber]]. We basically want to know how many ways the message can be decoded starting from the ith character.
That depends on two things-
1. The number of ways the i+1th character can be decoded (if we consider the ith char by itself)
2. The number of ways the i+2th character can be decoded (if we pair the ith char with i+1th)

So the equation is formed as `dp[i] = dp[i+1] + dp[i+2]`
Of course, there are a few constraints and base cases that have to handled too which the code will explain

### Code 
``` cpp
class Solution {
public:
    int dfs(int idx, string &s, int n, vector<int> &dp) {
        if(idx == n) return 1;
        if(s[idx] == '0') return 0;

        if(dp[idx] != -1) return dp[idx];

        // taking only a single character
        int res = dfs(idx + 1, s, n, dp);

        // if character can be paired with the next character
        if(idx < n - 1 && (s[idx] == '1' || s[idx] == '2' && s[idx + 1] < '7')) {
            res += dfs(idx + 2, s, n, dp);
        }

        dp[idx] = res;

        return res;
    }
    
    int numDecodings(string s) {
        int n = s.size();
        vector<int> dp(n + 1, -1);
        return dfs(0, s, n, dp);
    }
};
```