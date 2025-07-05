Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Reverse Reachability]], [[Graph]], [[DFS]]

In this [[DSA]] problem, There is an `m x n` rectangular island that borders both the **Pacific Ocean**and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n`integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return _a **2D list** of grid coordinates_ `result` _where_ `result[i] = [ri, ci]`_denotes that rain water can flow from cell_ `(ri, ci)` _to **both** the Pacific and Atlantic oceans_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

**Input:** heights = `[[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]`
**Output:**`[[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]`
**Explanation:** The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.

**Example 2:**

**Input:** heights = [[1]]
**Output:** [[0,0]]
**Explanation:** The water can flow from the only cell to the Pacific and Atlantic oceans.

**Constraints:**

- `m == heights.length`
- `n == heights[r].length`

## Approach  1- Using DFS and 2 visited grids
We will perform DFS in total 4 times. And while it may seem like a lot, it's not because we will only traverse one cell at max 2 times in the worst case.
We'll maintain a visited grid for each, the pacific and the atlantic and instead of checking from each cell to the ocean, we'll reverse that and check from the ocean to each cell.

So from each edge of the ocean we'll run a DFS to see which cells can be reached by each ocean. And the cell which can be reached by both the oceans will be pushed to the ans vector.

### Code 
``` cpp
typedef pair<int,int> state;

class Solution {
public:
    vector<vector<int>> visitedPacific;
    vector<vector<int>> visitedAtlantic;
    int n, m;

    vector<int> dx = {-1, 0, 1, 0};
    vector<int> dy = {0, 1, 0, -1};

    vector<state> getNeighbours(int i, int j) {
        vector<state> nbs;

        for(int nb = 0; nb < 4; nb++) {
            int newRow = dx[nb] + i;
            int newCol = dy[nb] + j; 
            if(
                (newRow < 0 || newRow >= n) ||
                (newCol < 0 || newCol >= m) 
            ) continue;
            nbs.push_back({newRow, newCol});
        }
        return nbs;
    }

    void dfs(int i, int j, vector<vector<int>> &heights, int vis) {
        if(vis == 0) visitedPacific[i][j] = 1;
        else if(vis == 1) visitedAtlantic[i][j] = 1;


        for(state n: getNeighbours(i, j)) {
            if((vis == 0 ? (!visitedPacific[n.first][n.second]) : (!visitedAtlantic[n.first][n.second])) && heights[n.first][n.second] >= heights[i][j]) {
                dfs(n.first, n.second, heights, vis);
            }
        }
    }

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        n = heights.size();
        m = heights[0].size();

        visitedPacific = vector<vector<int>> (n, vector<int> (m, 0));
        visitedAtlantic = vector<vector<int>> (n, vector<int> (m, 0));

        for(int j = 0; j < m; j++) { // first row
            if(!visitedPacific[0][j]) dfs(0, j, heights, 0);
        }

        for(int i = 0; i < n; i++) { // first col
            if(!visitedPacific[i][0]) dfs(i, 0, heights, 0);
        }

        for(int j = 0; j < m; j++) { // last row
            if(!visitedAtlantic[n-1][j]) dfs(n-1, j, heights, 1);
        }

        for(int i = 0; i < n; i++) { // last col
            if(!visitedAtlantic[i][m-1]) dfs(i, m-1, heights, 1);
        }

        vector<vector<int>> ans;

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(visitedPacific[i][j] && visitedAtlantic[i][j]) ans.push_back({i,j});
            }
        }

        return ans;  
    }
};
```