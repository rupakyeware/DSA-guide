Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Recursion]], [[Backtracking]], [[Dynamic Programming]]

In this [[DSA]] problem, given an `m x n` integers `matrix`, return _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

**Input:** matrix = [[9,9,4],[6,6,8],[2,1,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[1, 2, 6, 9]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

**Input:** matrix = [[3,4,5],[3,2,6],[2,2,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.

**Example 3:**

**Input:** matrix = [[1]]
**Output:** 1

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `0 <= matrix[i][j] <= 231 - 1`

## Approach 
This question didn't feel like a leetcode hard. I used ==form 2== of dp and approached it kinda like [[Problem- 300. Longest Increasing Subsequence]] but instead of looking for subsequence, I just checked if the current element is greater than the neighbouring elements.

So if the element I'm checking for is out of bounds, then just return 0. 
But if it's valid, then check which of the neighbours it's greater than and get the longest increasing path it can form with those neighbours if the current element is at the end of that path. Then the function will return the max of all valid directions.

In the main function, we will try to keep each element at the end of the path and see what's the max we can get and then return the max.

### Code 
```cpp
class Solution {
public:
    int m;
    int n;

    int rec(int i, int j, vector<vector<int>> &matrix, vector<vector<int>> &dp) {
        if((i < 0 || i >= m) || (j < 0 || j >= n)) return 0;

        if(dp[i][j] != -1) return dp[i][j];

        int ans = 1;
        if(i-1 >= 0 && matrix[i][j] > matrix[i-1][j]) ans = max(ans, rec(i-1, j, matrix, dp) + 1);
        if(i+1 < m && matrix[i][j] > matrix[i+1][j]) ans = max(ans, rec(i+1, j, matrix, dp) + 1);
        if(j-1 >= 0 && matrix[i][j] > matrix[i][j-1]) ans = max(ans, rec(i, j-1, matrix, dp) + 1);
        if(j+1 < n && matrix[i][j] > matrix[i][j+1]) ans = max(ans, rec(i, j+1, matrix, dp) + 1);

        return dp[i][j] = ans;
    }

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        m = matrix.size();
        n = matrix[0].size();
        vector<vector<int>> dp(m+1, vector<int> (n+1, -1));

        int maxi = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                maxi = max(maxi, rec(i, j, matrix, dp));
            }
        }

        return maxi;
    }
};
```