Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Multisource BFS]], [[Graph]], [[Reverse Reachability]]

In this [[DSA]] problem, you are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

**Input:** grid =`[[2,1,1],[1,1,0],[0,1,1]]`
**Output:** 4

**Example 2:**

**Input:** grid = `[[2,1,1],[0,1,1],[1,0,1]]`
**Output:** -1
**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** grid = `[[0,2]]`
**Output:** 0
**Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`.

## Approach 
This was a normal [[Multisource BFS]] problem but with an added condition of checking if there are any 1s left in the array after traversal.

### Code 

``` cpp
typedef pair<int,int> state;

class Solution {
public:
    int n, m;
    vector<vector<int>> visited;

    vector<int> dx = {-1,0,1,0};
    vector<int> dy = {0,1,0,-1};

    vector<state> getNeighbours(state cell, vector<vector<int>> &grid) {
        vector<state> nbs;

        for(int i = 0; i < 4; i++) {
            int newRow = cell.first + dx[i];
            int newCol = cell.second + dy[i];

            if(
                (newRow < 0 || newRow >= n) ||
                (newCol < 0 || newCol >= m) || 
                (grid[newRow][newCol] != 1)
            ) continue;
            
            nbs.push_back({newRow, newCol});
        }

        return nbs;
    }

    int bfs(vector<vector<int>>& grid) {
        queue<state> q;

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == 2) {
                    q.push({i,j});
                }
            }
        }

        int mins = 0;
        bool isChanged = false;
        while(!q.empty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                state curr = q.front();
                q.pop();

                grid[curr.first][curr.second] = 2;

                for(state nb: getNeighbours(curr, grid)) {
                    if(!visited[nb.first][nb.second]) {
                        if(!isChanged) isChanged = true;
                        q.push({nb.first, nb.second});
                        visited[nb.first][nb.second] = 1;
                    }
                }
            }
            if(isChanged) mins++;
            isChanged = false;
        }

        return mins;
    }

    int orangesRotting(vector<vector<int>>& grid) {
        n = grid.size();
        m = grid[0].size();
        visited = vector<vector<int>> (n, vector<int> (m, 0));
        int minsTaken = bfs(grid);

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == 1) return -1;
            }
        }

        return minsTaken;
    }
};
```