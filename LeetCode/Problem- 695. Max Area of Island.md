Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Recursion]], [[Backtracking]], [[Component Numbering]]

In this [[DSA]] problem, you are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return _the maximum **area** of an island in_ `grid`. If there is no island, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

**Input:** grid = 
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
```
**Output:** 6
**Explanation:** The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

**Input:** grid =
```
[[0,0,0,0,0,0,0,0]]
```
**Output:** 0

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

## Approach 
This problem was so easy. We just have to find the component with the largest size. We first form the components and then form a [[Frequency Map]]. I just kept a track of the max value at runtime so I won't have to iterate through the array again.

### Code 
```cpp
class Solution {
public:
    int m, n;

    void rec(int i, int j, int c, vector<vector<int>> &grid) {
        if((i < 0 || i >= m) || (j < 0 || j >= n)) return;
        if(grid[i][j] == 0 || grid[i][j] > 1) return;

        grid[i][j] = c;

        rec(i-1, j, c, grid);
        rec(i, j+1, c, grid);
        rec(i+1, j, c, grid);
        rec(i, j-1, c, grid);
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();

        int c = 2;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 1) {
                    rec(i, j, c, grid);
                    c++;
                }
            }
        }

        vector<int> frq(c);
        int maxi = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] > 1) {
                    frq[grid[i][j]]++;
                    maxi = max(maxi, frq[grid[i][j]]);
                }
            }
        }

        return maxi;
    }
};
```