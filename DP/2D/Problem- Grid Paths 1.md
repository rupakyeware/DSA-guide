Source: [[CSES]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, consider an n×nn×n grid whose squares may have traps. It is not allowed to move to a square with a trap.

Your task is to calculate the number of paths from the upper-left square to the lower-right square. You can only move right or down.

# Input

The first input line has an integer nn: the size of the grid.

After this, there are nn lines that describe the grid. Each line has nn characters: `.` denotes an empty cell, and `*` denotes a trap.

# Output

Print the number of paths modulo 109+7109+7.

# Constraints

- 1≤n≤10001≤n≤1000

# Example

Input:

```
4
....
.*..
...*
*...
```
Output:

`3`

## Approach 1- Top Down
We want to perform a [[DFS]] on the table with 2 branches for each cell- one going down and one going up and we will do this recursively. After getting the number of paths possible for a cell, we store it in the dp table for memoization. The code is pretty self explanatory. 

Also we have to mod it with 1e9 + 7 at each step.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

const int MOD = 1e9 + 7;

  

int dfs(vector<vector<char>> &grid, vector<vector<int>> &dp, int row, int col, int n) {

if(dp[row][col] != -1) return dp[row][col]; // if we've already visited this loc

  

int paths = 0;

  

if(row + 1 < n && grid[row+1][col] != '*') paths = ((paths % MOD) + (dfs(grid, dp, row + 1, col, n) % MOD)) % MOD; // explore down if not *

if(col + 1 < n && grid[row][col+1] != '*') paths = ((paths % MOD) + (dfs(grid, dp, row, col + 1, n) % MOD)) % MOD; // explore right if not *

  

dp[row][col] = paths; // write to dp table

return paths;

}

  

void solve() {

int n; cin >> n;

vector<vector<char>> grid(n, vector<char> (n));

vector<vector<int>> dp(n, vector<int> (n, -1));

  

for(int i = 0; i < n; i++) {

for(int j = 0; j < n; j++) {

cin >> grid[i][j];

}

}

  

dp[n-1][n-1] = (grid[n-1][n-1] == '*' ? 0: 1);

  

cout << (grid[0][0] == '*' ? 0 : dfs(grid, dp, 0, 0, n)) << endl;

}

  

signed main() {

ios_base::sync_with_stdio(false);

cin.tie(NULL); cout.tie(NULL);

  

int _t = 1;

// cin >> _t;

  

while(_t--) solve();

return 0;

}
```

## Approach 2- Bottom Up
In this approach, we will just fill the dp table based on one question- ==In how many ways can we reach this cell?==
The answer to that is the sum of the dp cell above it and the dp cell to the left. Of course, this is keeping the blocks in mind.
The base cases will be the first row and first column.
We will fill the first row with 1s until we hit a block and the same for the first column.
Then we will fill the rest of the dp table. 

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

const int MOD = 1e9 + 7;

  

void solve() {

int n; cin >> n;

vector<vector<char>> grid(n, vector<char> (n)); // path grid

vector<vector<int>> dp(n, vector<int> (n, 0)); // dp table

  

for(int i = 0; i < n; i++) {

for(int j = 0; j < n; j++) {

cin >> grid[i][j];

}

}

  

// set the base cases for 1st row

for(int i = 0; i < n; i++) {

if(grid[0][i] == '*') break;

dp[0][i] = 1;

}

  

// set the base cases for 1st col

for(int i = 0; i < n; i++) {

if(grid[i][0] == '*') break;

dp[i][0] = 1;

}

  

// fill the dp table

for(int i = 1; i < n; i++) {

for(int j = 1; j < n; j++) {

if(grid[i][j] == '*') continue;

  

int paths = 0;

if(grid[i][j-1] != '*') paths = (paths + dp[i][j-1]) % MOD; // cell to the left

if (grid[i-1][j] != '*') paths = (paths + dp[i-1][j]) % MOD; // cell above

  

dp[i][j] = paths;

}

}

  

cout << dp[n-1][n-1] << endl;

}

  

signed main() {

ios_base::sync_with_stdio(false);

cin.tie(NULL); cout.tie(NULL);

  

int _t = 1;

// cin >> _t;

  

while(_t--) solve();

return 0;

}
```