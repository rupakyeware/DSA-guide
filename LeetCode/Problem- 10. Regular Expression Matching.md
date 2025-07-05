Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Dynamic Programming]], [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.​​​​
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

**Example 2:**

**Input:** s = "aa", p = "a*"
**Output:** true
**Explanation:** '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

**Example 3:**

**Input:** s = "ab", p = ".*"
**Output:** true
**Explanation:** ".*" means "zero or more (*) of any character (.)".

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= p.length <= 20`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'.'`, and `'*'`.
- It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

## Approach 
Now this felt like an actually hard problem. Identifying the form, which was ==form 3==, wasn't difficulty. The actual tricky part was figuring out which transition to perform for which condition. I was able to figure out that there would be only 1 main decision- if the char is a * or not. Because the process for . and normal char was the same if the normal chars were matching.

I had to take the help of chatgpt to figure out the issues and for [[debugging]] too.

Basically, first we find out if there is a match between the characters (. also counts as match).
If there isn't a match, we first check if the next char is a * and if it is, we can do 2 things-
- if match then move just i ahead (1 or more occurence)
- else move j ahead by 2 (0 occurence)
If the next char isn't a star, we handle it like a normal character-
- if match then move both chars ahead by 1.
And then return whatever result we get.

### Code 
```cpp
class Solution {
public:
    string A, B;
    int sA, sB;

    bool rec(int i, int j, vector<vector<int>> &dp) {
        if(i > sA || j > sB) return false;
        if(dp[i][j] != -1) return dp[i][j];
        if(j == sB) return dp[i][j] = (i == sA);

        // transition
        bool ans;
        bool match = (i < sA && (A[i] == B[j] || B[j] == '.'));

        if(j + 1 < sB && B[j+1] == '*') {
            ans = rec(i, j+2, dp) || (match && rec(i+1, j, dp));
        }
        else {
            ans = (match && rec(i+1, j+1, dp));
        }
        

        // return
        return dp[i][j] = ans;
    }

    bool isMatch(string s, string p) {
        A = s; B = p;
        sA = s.size(); sB = p.size();
        vector<vector<int>> dp(sA+1, vector<int> (sB+1, -1));

        return rec(0, 0, dp);
    }
};
```