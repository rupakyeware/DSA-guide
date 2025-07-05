Source: [[Leetcode]]
Topics: [[Recursion]], [[Backtracking]]
Difficulty: [[Hard]]
I don't know why this question is categorized as hard. It's an upper medium. 

In this [[DSA]] question called **n-queens**, the problem is of placing `n` queens on an `n x n`chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4
**Output:** `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1
**Output:** `[["Q"]]`

**Constraints:**

- `1 <= n <= 9`

## Approach 
We are basically going to brute force the answer while backtracking to drastically reduce the number of function calls we make.

We will use the LCCM property of backtracking where the level will be the rows the queens will be placed at, the choice will be the column, the check will be to check if no other queens have been placed in the same row, column and diagonal and the move will be to place the queen.

The placed queens will be tracked using a global int vector.

We will try to place queens at each column for every row if it's possible to place a queen there. If a queen can be placed, we place it and call the function again for the next level. But if it can't be placed anywhere, the control automatically goes back to the previous function call's next iteration for column to try.

If we reach row number n (0 to n - 1 will be the rows on the chessboard), then we have successfully managed to place n queens so we will append the answer in graphical format to the ans vector of vector of strings.

And then the control goes back to trying more solutions. 

### Code 
``` cpp
#define MAX 10

  

class Solution {

public:

vector<int> queensPlaced; // how queens have been placed curently

vector<vector<string>> ans; // all solutions

  

Solution(): queensPlaced(MAX, -1) {} // initialize MAX rows with -1 in vector

  

bool check(int level, int col) {

for(int i = 0; i < level; i++) {

if(queensPlaced[i] == col || abs(i - level) == abs(queensPlaced[i] - col)) {

return false;

}

}

return true;

}

  

void placeQueen(int n, int level) {

// if all queens have been placed

if (level == n) {

string init(n, '.'); // init string of size n with n dots

vector<string> currAns(n, init); // initialize n string with n dots in vector

for(int i = 0; i < n; i++) {

currAns[i][queensPlaced[i]] = 'Q'; // at ith row, mark where queen is placed

}

ans.push_back(currAns); // add curr ans to final ans vector

}

  

for(int i = 0; i < n; i++) {

if(check(level, i)) { // if queen can be placed in ith col

queensPlaced[level] = i; // place queen

placeQueen(n, level + 1); // call recursive function for next level

}

queensPlaced[level] = -1; // unplace queen

}

}

vector<vector<string>> solveNQueens(int n) {

placeQueen(n, 0);

return ans;

}

};
```
