Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Array]]

In this [[DSA]] problem, given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
**Output:** `[[1,0,1],[0,0,0],[1,0,1]]`

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = [[`0,1,2,0],[3,4,5,2],[1,3,1,5`]]
**Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

**Follow up:**

- A straightforward solution using `O(mn)` space is probably a bad idea.
- A simple improvement uses `O(m + n)` space, but still not the best solution.
- Could you devise a constant space solution?

## Approach 
This is my code that uses O(m x n) time and O(m + n) space. We will maintain two boolean vectors of size m and n respectively. The first will store if the current row has a zero and the second will store if the current column has a zero. We'll first perform a pass over the entire matrix to mark the appropriate rows and columns. 

Then in the second pass, we'll see if either the row or column is marked as true for the particular cell and if it is, we change it to 0.

### Code 
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int r = matrix.size();
        int c = matrix[0].size();
        
        vector<bool> row0 = vector<bool> (r, false);
        vector<bool> col0 = vector<bool> (c, false);
        
        for(int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                if(matrix[i][j] == 0) {
                    row0[i] = true;
                    col0[j] = true;
                }
            }
        }
        
        for(int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                if(row0[i] || col0[j]) matrix[i][j] = 0;
            }
        }
    }
};
```