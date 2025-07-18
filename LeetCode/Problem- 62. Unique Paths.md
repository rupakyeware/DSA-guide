Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]], [[Dynamic Programming]]

In this [[DSA]] problem, there is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

**Input:** m = 3, n = 7
**Output:** 28

**Example 2:**

**Input:** m = 3, n = 2
**Output:** 3
**Explanation:** From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

**Constraints:**

- `1 <= m, n <= 100`

## Approach 
This was very easy to solve using ==form 2== of dp. 
Basically, we want know know `in how many ways can we reach arr[i][j]`
So our dp function will be the same where
`dp(i, j) -> the number of ways to reach arr[i][j] from arr[0][0]`
And that will depend on the number of ways to reach the cell above the current cell and the cell to the left.
`dp(i, j) = dp(i-1,j) + dp(i, j-1)`

### Code 

``` cpp
class Solution {
public:
    int rows, cols;
    int rec(vector<vector<int>> &dp, int i, int j) {
        // pruning
        if((i < 0 || i >= rows) || (j < 0 || j >= cols)) return 0;

        // base case
        if(i == 0 && j == 0) return 1;

        // dp check
        if(dp[i][j] != -1) return dp[i][j];

        // transition
        int ans = rec(dp, i-1, j) + rec(dp, i, j-1);

        // save and return
        return dp[i][j] = ans;
    }

    int uniquePaths(int m, int n) {
        rows = m; cols = n;
        vector<vector<int>> dp(m+1, vector<int> (n+1, -1));
        return rec(dp, m-1, n-1);
    }
};
```
