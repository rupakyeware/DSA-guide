Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Recursion]], [[Dynamic Programming]], [[Backtracking]]

In this [[DSA]] problem, given two strings s and t, return _the number of distinct_ **_subsequences_** _of_ s_which equals_ t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example 1:**

**Input:** s = "rabbbit", t = "rabbit"
**Output:** 3
**Explanation:**
As shown below, there are 3 ways you can generate "rabbit" from s.
`**rabb**b**it**`
`**ra**b**bbit**`
`**rab**b**bit**`

**Example 2:**

**Input:** s = "babgbag", t = "bag"
**Output:** 5
**Explanation:**
As shown below, there are 5 ways you can generate "bag" from s.
`**ba**b**g**bag`
`**ba**bgba**g**`
`**b**abgb**ag**`
`ba**b**gb**ag**`
`babg**bag**`

**Constraints:**

- `1 <= s.length, t.length <= 1000`
- `s` and `t` consist of English letters.

## Approach-
I don't know why this problem is marked as Hard. This felt like a medium problem where to just have to find the number of subsequences being formed. 

Anyways, I used a combination of ==form 1 ==and ==form 3== where I'm placing a pointer on string s and t each and then doing a pick / not pick recursive call. The answer of one function call will the pick + not pick basically.

Like I said. Easy peasy. Took me a total of 31 minutes to come up with the idea and to get the solution accepted. I was failing one test case but dry running that failing test case helped me find my mistake. My [[debugging]] skills have gotten much better since I started doing DSA.
### Code 
```cpp
class Solution {
public:
    string A, B;

    int rec(int i, int j, vector<vector<int>> &dp) {
        if(j == B.size()) return 1;
        if((B.size()-j > A.size()-i)||(i == A.size())) return 0;
        
        if(dp[i][j] != -1) return dp[i][j];

        int ans = rec(i+1, j, dp);
        if(A[i] == B[j]) {
            int pick = rec(i+1, j+1, dp);
            ans += pick; 
        }

        return dp[i][j] = ans;
    }

    int numDistinct(string s, string t) {
        if(s.size() < t.size()) return 0;

        A = s;
        B = t;
        vector<vector<int>> dp(A.size() + 1, vector<int> (B.size() + 1, -1));

        return rec(0, 0, dp);
    }
};
```