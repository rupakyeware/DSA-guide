Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[DFS]]

In this [[DSA]] problem, given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

**Input:** grid = `[[1,3,1],[1,5,1],[4,2,1]]`
**Output:** 7
**Explanation:** Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

**Example 2:**

**Input:** grid = `[[1,2,3],[4,5,6]]`
**Output:** 12

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

## Approach 
It's a basic path finding approach using [[Dynamic Programming]].

### Code 
``` cpp
class Solution {
public:
    vector<vector<int>> dp;
    int m, n;

    int rec(int i, int j, vector<vector<int>> &grid) {
        if((i < 0 || i >= m) || (j < 0 || j >= n)) return 100100;

        if(i == m-1 && j == n-1) return grid[i][j];

        if(dp[i][j] != -1) return dp[i][j];

        int ans = grid[i][j] + min(rec(i+1, j, grid), rec(i, j+1, grid));

        return dp[i][j] = ans;
    }

    int minPathSum(vector<vector<int>>& grid) {
        m = grid.size(); n = grid[0].size();
        dp = vector<vector<int>> (m, vector<int> (n, -1));

        return rec(0, 0, grid);
    }
};
```