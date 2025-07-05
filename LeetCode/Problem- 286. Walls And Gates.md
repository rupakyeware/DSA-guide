Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Multisource BFS]]

In this [[DSA]] problem, you are given a m×n 2D `grid` initialized with these three possible values:

1. `-1` - A water cell that _can not_ be traversed.
2. `0` - A treasure chest.
3. `INF` - A land cell that _can_ be traversed. We use the integer `2^31 - 1 = 2147483647` to represent `INF`.

Fill each land cell with the distance to its nearest treasure chest. If a land cell cannot reach a treasure chest then the value should remain `INF`.

Assume the grid can only be traversed up, down, left, or right.

Modify the `grid` **in-place**.

**Example 1:**

```java
Input: [
  [2147483647,-1,0,2147483647],
  [2147483647,2147483647,2147483647,-1],
  [2147483647,-1,2147483647,-1],
  [0,-1,2147483647,2147483647]
]

Output: [
  [3,-1,0,1],
  [2,2,1,-1],
  [1,-1,2,-1],
  [0,-1,3,4]
]
```

**Example 2:**

```java
Input: [
  [0,-1],
  [2147483647,2147483647]
]

Output: [
  [0,-1],
  [1,2]
]
```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `grid[i][j]` is one of `{-1, 0, 2147483647}`

## Approach 1- [[DFS]]
This is the approach that I came up with which has a time complexity of O(n * m)^2
For every gate we encounter, we perform a dfs and update the distance of a neighbouring cell if the distance to it from the current cell is less than it was before (meaning we found a closer gate). The downside of this approach is the revisiting of cells again and again 

### Code 
``` cpp
typedef pair<int,int> state;

class Solution {
public:
    int n, m;
    vector<int> dx = {0,-1,0,1};
    vector<int> dy = {-1,0,1,0};

    vector<state> getNeighbours(state cell, vector<vector<int>> &grid) {
        vector<state> neighbours;

        for(int i = 0; i < 4; i++) {
            int newRow = cell.first + dx[i];
            int newCol = cell.second + dy[i];

            if(
                (newRow < 0 || newRow >= n) ||
                (newCol < 0 || newCol >= m) ||
                (grid[newRow][newCol] == -1)
            ) continue;
            neighbours.push_back({newRow,newCol});
        }

        return neighbours;
    }

    void dfs(vector<vector<int>> &grid, state currCell) {
        for(state cell: getNeighbours(currCell, grid)) {
            if(grid[cell.first][cell.second] > grid[currCell.first][currCell.second] + 1) {
                grid[cell.first][cell.second] = grid[currCell.first][currCell.second] + 1;
                dfs(grid, cell);
            }
        }
    }

    void islandsAndTreasure(vector<vector<int>>& grid) {
        state st;
        n = grid.size();
        m = grid[0].size();
        
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == 0) {
                    st.first = i;
                    st.second = j;
                    grid[st.first][st.second] = 0;
                    dfs(grid, st);
                }
            }
        }
    }
};
```

## Approach 2- Multisource BFS 
This is something new that I learned from Neetcode. 
We use a queue like a normal BFS algorithm and update the distances to the neighbours of all 0s, then all the neighbours of 1s, and so on.

So we use a for loop inside our while(!q.empty()) to get the number of elements we have to update.
Then we update their distance using a dist counter while also pushing the neighbours of those cells into the queue if they haven't been visited before.
After the for loop finishes executing, we increment the value of dist.

It's a very genius approach.

### Code 
``` cpp
typedef pair<int,int> state;

class Solution {
public:
    int n, m;
    vector<vector<int>> visited;
    vector<int> dx = {0,-1,0,1};
    vector<int> dy = {-1,0,1,0};

    vector<state> getNeighbours(state cell, vector<vector<int>> &grid) {
        vector<state> neighbours;

        for(int i = 0; i < 4; i++) {
            int newRow = cell.first + dx[i];
            int newCol = cell.second + dy[i];

            if(
                (newRow < 0 || newRow >= n) ||
                (newCol < 0 || newCol >= m) ||
                (grid[newRow][newCol] == -1)
            ) continue;
            neighbours.push_back({newRow,newCol});
        }

        return neighbours;
    }

    void bfs(vector<vector<int>> &grid) {
        queue<state> q;

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == 0) {
                    q.push({i,j});
                    visited[i][j] = 1;
                }
            }
        }

        int dist = 0;
        while(!q.empty()) {
            int len = q.size();
            for(int i = 0; i < len; i++) {
                state cell = q.front();
                q.pop();
                grid[cell.first][cell.second] = dist;

                for(state s: getNeighbours(cell, grid)) {
                    if(!visited[s.first][s.second]) {
                        q.push(s);
                        visited[s.first][s.second] = 1;
                    }
                }
            }
            dist++;
        }
    }

    void islandsAndTreasure(vector<vector<int>>& grid) {
        n = grid.size();
        m = grid[0].size();
        visited = vector<vector<int>> (n, vector<int> (m, 0));
        bfs(grid);
    }
};
```
