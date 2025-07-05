Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Djikstra]], [[BFS]], [[Graph]]

In this [[DSA]] problem, you are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return _the least time until you can reach the bottom right square_ `(n - 1, n - 1)` _if you start at the top left square_ `(0, 0)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

**Input:** grid = `[[0,2],[1,3]]`
**Output:** 3
Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)

**Input:** grid = `[[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]`
**Output:** 16
**Explanation:** The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 50`
- `0 <= grid[i][j] < n2`
- Each value `grid[i][j]` is **unique**.

## Approach 
The key to this problem is recognizing that we need to use a djikstra-like approach not to find the shortest path, but the shortest time to reach the end. And that took me some dry running to figure out the algorithm but I was able to do it myself completely (yay).

So since we can travel infinite distance in 0 time (im fast af boi), we don't keep a track of distances in the min heap. Instead, we keep a track of the minimum elevation. And t will be the max of the popped element and the value already in t. Which, if we want to define more accurately, t is the maximum minimum elevation we have seem up until now, but that makes it a bit difficulty to understand. Simply said, t is going to be how high the rain water has reached. 

The code will do a better job of explaining this than I. And the code really is pretty simple.

### Code 
```cpp
class Solution {
public:
    int n;
    vector<vector<bool>> visited;
    vector<int> dx = {-1, 0, 1, 0};
    vector<int> dy = {0, 1, 0, -1};

    vector<pair<int,int>> getNeighbours(int x, int y) {
        vector<pair<int,int>> nbs;

        for(int i = 0; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];

            if(
                (newX < 0 || newY < 0) ||
                (newX >= n || newY >= n)
            ) continue;

            nbs.push_back({newX, newY});
        }

        return nbs;
    }

    int bfs(vector<vector<int>> &grid) {
        int t = 0;
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> pq;
        pq.push({grid[0][0], {0,0}});

        while(!pq.empty()) {
            auto [time, coord] = pq.top();
            pq.pop();

            if(visited[coord.first][coord.second]) continue;
            else visited[coord.first][coord.second] = 1;

            t = max(t, time);

            if(coord.first == n-1 && coord.second == n-1) return t;

            for(auto &[x, y]: getNeighbours(coord.first, coord.second)) {
                if(!visited[x][y]) pq.push({grid[x][y], {x, y}});
            }
        }


        return 0;
    }

    int swimInWater(vector<vector<int>>& grid) {
        n  = grid.size();
        visited = vector<vector<bool>> (n, vector<bool> (n,0));

        return bfs(grid);
    }
};
```