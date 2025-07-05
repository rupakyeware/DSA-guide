Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Reverse Reachability]], [[Multisource BFS]]

In this [[DSA]] problem, you are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

To capture a **surrounded region**, replace all `'O'`s with `'X'`s **in-place**within the original board. You do not need to return anything.

**Example 1:**

**Input:** board = `[["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`

**Output:** `[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]`

**Explanation:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = `[["X"]]`

**Output:** `[["X"]]`

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.

## Approach 
This was a nice problem. It took me some thinking to figure out that it's a [[Reverse Reachability]] problem. Also, the terrible problem description didn't help.

So we will first push all the Os in the first row, first column and last row, last column and then perform a [[Multisource BFS]] on it to visit all the neighbouring Os of the Os in the [[Queue]]. These are going to be the cells which will NOT be flipped to Xs.

So after the entire graph has been traversed, we will go through the graph again and flip all the Os which haven't been visited to Xs. 

### Code 
```cpp
typedef pair<int,int> state;

class Solution {
public:
    int n, m;
    vector<vector<int>> visited;    

    vector<int> dx = {-1, 0, 1, 0};
    vector<int> dy = {0, 1, 0, -1};

    vector<state> getNeighbours(state curr, vector<vector<char>> &board) {
        vector<state> nbs;

        for(int i = 0; i < 4; i++) {
            int r = curr.first + dx[i];
            int c = curr.second + dy[i];

            if(
                (r < 0 || r >= n) ||
                (c < 0 || c >= m) ||
                (board[r][c] == 'X')
            ) continue;

            nbs.push_back({r, c});
        }

        return nbs;
    }

    void bfs(vector<vector<char>> &board) {
        queue<state> q;

        for(int i = 0; i < m; i++) { // first and last row
            if(board[0][i] == 'O') q.push({0,i});
            if(board[n-1][i] == 'O') q.push({n-1,i});
        }

        for(int j = 0; j < n; j++) { // first and last col
            if(board[j][0] == 'O') q.push({j,0});
            if(board[j][m-1] =='O') q.push({j,m-1});
        }

        while(!q.empty()) {
            state curr = q.front();
            q.pop();

            if(visited[curr.first][curr.second]) continue;
            visited[curr.first][curr.second] = 1;

            for(state nb: getNeighbours(curr, board)) {
                if(!visited[nb.first][nb.second]) {
                    q.push({nb.first, nb.second});
                }
            }
        }
    }

    void solve(vector<vector<char>>& board) {
        n = board.size();
        m = board[0].size();

        visited = vector<vector<int>> (n, vector<int> (m, 0));

        bfs(board);

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                // cout << visited[i][j] << " ";
                if(board[i][j] == 'O' && !visited[i][j]) board[i][j] = 'X';
            } 
            // cout << endl;
        }
    }
};
```