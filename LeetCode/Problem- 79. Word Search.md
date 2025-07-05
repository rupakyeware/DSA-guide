Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]], [[DFS]]

In this [[DSA]] problem, given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word`_exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "ABCCED"
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "SEE"
**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "ABCB"
**Output:** false

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

## Approach 1- Using a 2D boolean array to track visited
The main approach is pretty straightforward. I was able to somewhat come up with it but I got rusty because of a 5 day gap. The only challenging part was figuring out how to keep a track of which nodes we have visited.

This approach uses a 2D boolean vector to do that where we essentially mark the ith by jth element as true when we visit it.

But if the current node doesn't match with our requirements, we just return false.

### Code 
``` cpp
class Solution {

public:

vector<vector<bool>> visited;

int R, C;

  

bool exist(vector<vector<char>>& board, string word) {

R = board.size();

C = board[0].size();

  

visited = vector<vector<bool>> (R, vector<bool> (C, false));

  

for(int i = 0; i < R; i++) {

for(int j = 0; j < C; j++) {

if(dfs(board, word, i, j, 0)) return true;

}

}

  

return false;

}

  

bool dfs(vector<vector<char>> &board, string &word, int r, int c, int idx) {

if(idx == word.size()) return true;

  

if(

(r < 0 || r >= R) || // row out of bounds

(c < 0 || c >= C) || // col out of bounds

(board[r][c] != word[idx]) || // invalid path

(visited[r][c]) // already visited this before

) return false;

  

visited[r][c] = true;; // we have visited the current valid node

  

bool isPossible = (

dfs(board, word, r - 1, c, idx + 1) || // up

dfs(board, word, r, c + 1, idx + 1) || // right

dfs(board, word, r + 1, c, idx + 1) || // down

dfs(board, word, r, c - 1, idx + 1) // left

);

  

visited[r][c] = false; // erase current node from visited

  

return isPossible;

}

};
```

## Approach 2- Manipulating the main board itself
In this particular problem, the max rows and cols can only be up to 6. But if they were more, then the previous approach wouldn't be the best as it has a linear space complexity. However, here, we aren't utilizing any extra space so the space complexity is O(1).

Here, we just change the value of the particular nodes we've visited to '#' and that's how we keep a track of which nodes we've visited.
``` cpp
class Solution {

public:

vector<vector<bool>> visited;

int R, C;

  

bool exist(vector<vector<char>>& board, string word) {

R = board.size();

C = board[0].size();

  

for(int i = 0; i < R; i++) {

for(int j = 0; j < C; j++) {

if(dfs(board, word, i, j, 0)) return true;

}

}

  

return false;

}

  

bool dfs(vector<vector<char>> &board, string &word, int r, int c, int idx) {

if(idx == word.size()) return true;

  

if(

(r < 0 || r >= R) || // row out of bounds

(c < 0 || c >= C) || // col out of bounds

(board[r][c] != word[idx]) || // invalid path

(board[r][c] == '#') // already visited this before

) return false;

  

char temp = board[r][c]; // store current value temporarily

board[r][c] = '#'; // we have visited the current valid node

  

bool isPossible = (

dfs(board, word, r - 1, c, idx + 1) || // up

dfs(board, word, r, c + 1, idx + 1) || // right

dfs(board, word, r + 1, c, idx + 1) || // down

dfs(board, word, r, c - 1, idx + 1) // left

);

  

board[r][c] = temp; // reset value of current node

  

return isPossible;

}

};
```