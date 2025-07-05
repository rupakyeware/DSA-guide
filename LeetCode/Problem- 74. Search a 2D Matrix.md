Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]]

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.

You must write a solution in `O(log(m * n))` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

**Input:** matrix = `[[1,3,5,7],[10,11,16,20],[23,30,34,60]]`, target = 3
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

**Input:** matrix = `[[1,3,5,7],[10,11,16,20],[23,30,34,60]]`, target = 13
**Output:** false

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-104 <= matrix[i][j], target <= 104`

## Approach - Double binary search 
I was able to come up with the answer for this pretty easily since binary search has already been done quite extensively in [[AlgoZenith]]. But it still took me a couple wrong answer to get to the right one because I made a few silly mistakes.

Essentially, we want to perform two binary searches to find the target.
The first one, to find the correct row. The correct row is the row with it's first element having a value that's <= target.
If all the 1st values are greater than target, it means that the target doesn't exist so we can return false.

Else if we found the right row, we just perform a regular binary search on it to get the right answer if it exists, else we return a -1.

### Code 
``` cpp
class Solution {

public:

bool checkRow(int target, int curr) {

return curr <= target;

}

  

bool searchMatrix(vector<vector<int>>& matrix, int target) {

int n = matrix.size();

int m = matrix[0].size();

  

// Find the target's row using binary search

int lo = 0, hi = n - 1, ans = -1;

while (lo <= hi) {

int mid = lo + (hi - lo) / 2;

if(checkRow(target, matrix[mid][0])) {

ans = mid;

lo = mid + 1;

}

else hi = mid - 1;

}

  

// If all values are greater than 0, return false

if(ans < 0) return false;

int targetRow = ans;

// Find the target's col using binary search

lo = 0, hi = m - 1, ans = -1;

while(lo <= hi) {

int mid = lo + (hi - lo) / 2;

int val = matrix[targetRow][mid];

cout << mid << ":" << val << endl;

if(val < target) lo = mid + 1;

else if(val > target) hi = mid - 1;

else {

ans = mid;

break;

}

}

return (ans < 0 ? false : true);

}

};
```
