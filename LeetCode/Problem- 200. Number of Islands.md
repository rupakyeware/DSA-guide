Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Component Numbering]], [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = 
```
[
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```
**Output:** 1

**Example 2:**

**Input:** grid = 
```
[
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
**Output:** 3

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Approach 
We just have to perform a lower level of component numbering where we're not actually numbering the components, just increasing the counter. If we are at a valid unvisited piece of land, we mark it as water and then explore its neighbours. 
After visiting all its neighbours, we increment the counter.

### Code 

``` cpp
class Solution {
public:
    int m, n;
    void rec(int i, int j, vector<vector<char>>& grid) {
        if((i < 0 || i >= m) || (j < 0 || j >= n) || grid[i][j] == '0') return;

        grid[i][j] = '0';
        rec(i-1, j, grid);
        rec(i, j+1, grid);
        rec(i+1, j, grid);
        rec(i, j-1, grid);
    }

    int numIslands(vector<vector<char>>& grid) {
        m = grid.size();
        n = grid[0].size();

        int c = 1;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == '1') {
                    rec(i, j, grid);
                    c++;
                }
            }
        }

        return c-1;
    }
};
```
