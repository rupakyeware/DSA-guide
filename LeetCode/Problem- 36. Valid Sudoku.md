Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Unordered Map]], [[2 pointers]], [[Frequency Map]]

For this [[DSA]] problem, Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

Example 1-
![[Problem- 36. Valid Sudoku Img.png]]
Output - True

## Approach Used-
Perform 3 checks on the entire board-
1. Check if all rows are valid
2. Check if all columns are valid
3. Check if all grids are valid

Checking if the rows and columns are valid is easy. Just iterate through the board while keeping a frequency map and if any element gets incremented more than once, return false

Checking grids is a bit more challenging but still pretty easy.
The approach I used is I [[2 pointers]] to keep a track of the start row and start column. I iterate through 9 grids while incrementing the start column by 3 every time unless the end of the columns has been reached. Then I increment the start row by 3 and start checking for all grids horizontally again. 
Essentially, I'm iterating through each grid horizontally to see if they are valid or not. The same logic of a frequency map is used.

## Code
```
class Solution {

public:

    bool isValidSudoku(vector<vector<char>>& board) {

        // Check if all rows are valid

        bool rowValid = true;

        for(int i = 0; i < 9; i++) {

            unordered_map<int, int> umap;

            for(int j = 0; j < 9; j++) {

                if(board[i][j] == '.') continue;

                umap[board[i][j]]++;

                if(umap[board[i][j]] > 1) {

                    rowValid = false;

                    break;

                }

            }

            if(!rowValid) break;

        }

        if(!rowValid) return false;

  

        // Check if all columns are valid

        bool colValid = true;

        for(int i = 0; i < 9; i++) {

            unordered_map<int, int> umap;

            for(int j = 0; j < 9; j++) {

                if(board[j][i] == '.') continue;

                umap[board[j][i]]++;

                if(umap[board[j][i]] > 1) {

                    colValid = false;

                    break;

                }

            }

            if(!colValid) break;

        }

        if(!colValid) return false;

  

        // Check if all grids are valid

        bool gridValid = true;

        int start_r = 0;

        int start_c = 0;

        for(int g = 0; g < 9; g++) {

            unordered_map<int, int> umap;

            for(int i = start_r; i < start_r + 3; i++) {

                for(int j = start_c; j < start_c + 3; j++) {

                    if(board[i][j] == '.') continue;

                    umap[board[i][j]]++;

                    if(umap[board[i][j]] > 1) {

                        gridValid = false;

                        break;

                    }

                }

                if(!gridValid) break;

            }

            start_c += 3;

            if(start_c == 9) {

                start_r += 3;

                start_c = 0;

            }

        }

        if(!gridValid) return false;

        return true;

    }

};
```
