Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Recursion]], [[Backtracking]] 

In this [[DSA]] problem, we have to write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3`sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = `[["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]`

**Output:** `[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]`
**Explanation:** The input board is shown above and the only valid solution is shown below:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit or `'.'`.
- It is **guaranteed** that the input board has only one solution.

## Approach 
This was pretty tricky. In my first go, I wasn't able to come up with the intuition for the problem or even understand the code written by the [[AlgoZenith]] instructor. Although, his code was a bit more unnecessarily complex. So I kept this problem aside for 2-3 days and came back to it and go it completely right on the first try except for one small silly mistake where instead of passing the reference of the board in the check function, I passed the value. So that resulted in a TLE.

I just had to understand and visualize the recursion tree properly. Then the code just wrote itself. 

So the approach is pretty simple. If a cell is empty at a particular row and column, we want to try to place all possible numbers between 1 to 9 as long as they are valid. If we get a true, it means that the current number was place correctly. Which means the number before this was correct. Which means the number before that was correct and so on. So if we get 1 right, we get all right. It's a chain reaction.

The check function is pretty straightforward. The grid check's formula is interesting. The intuition for that came from the instructor. 

### Code 
``` cpp
class Solution {

public:

bool check(vector<vector<char>> &board, int row, int col, char num) {

// check if row is valid

for(int i = 0; i < 9; i++) {

if(board[row][i] == num) return false;

}

  

// check if col is valid

for(int i = 0; i < 9; i++) {

if(board[i][col] == num) return false;

}

  

// check if grid is valid

int rowStart = (row / 3) * 3;

int colStart = (col / 3) * 3;

int rowEnd = rowStart + 3;

int colEnd = colStart + 3;

for(int i = rowStart; i < rowEnd; i++) {

for(int j = colStart; j < colEnd; j++) {

if(board[i][j] == num) return false;

}

}

  

return true;

}

  

bool placeNumber(vector<vector<char>> &board, int row, int col) {

// we have reached the end of this row so move to next row

if(col == 9) {

row = row + 1;

col = 0;

}

// we have successfuly solved the grid

if(row == 9) return true;

// current cell is already filled so solve next empty cell

if(board[row][col] != '.') return placeNumber(board, row, col + 1);

  

for(char i = '1'; i <= '9'; i++) {

if(check(board, row, col, i)) {

board[row][col] = i;

if(placeNumber(board, row, col + 1)) {

return true;

}

board[row][col] = '.';

}

}

  

// board is unsolvable at current state so backtrack

return false;

}

  

void solveSudoku(vector<vector<char>>& board) {

placeNumber(board, 0, 0);

}

};
```