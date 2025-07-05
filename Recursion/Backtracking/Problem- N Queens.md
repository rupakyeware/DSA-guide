Source: [[AlgoZenith]]
Topics: [[Backtracking]], [[Recursion]]
Difficulty: [[Medium]]

In this [[DSA]] problem, you are given n, the number of queens to be placed on an n x n chess board. Return the number of queens that can be placed along with the solutions.

**Example 1:**

**Input:** n = 4
**Output:** 
1 3 0 2
2 0 3 1
2

**Example 2:**

**Input:** n = 5
**Output:** 
0 2 4 1 3
0 3 1 4 2
1 3 0 2 4
1 4 2 0 3
2 0 3 1 4
2 4 1 3 0
3 0 2 4 1
3 1 4 2 0
4 1 3 0 2
4 2 0 3 1
10

## Approach 
We want to use backtracking to try to solve this level wise using backtracking.

We place one queen at the ith place on a level. Then call the same function again for the next level. If the next level is n, we return 0. Once a value is returned, we place the queen at the next location on the same level and repeat.

But before we 'place' a queen, we have to see if it's even possible to place a queen there using a check function that essentially returns true or false depending upon the validity of the board if we place a queen at the ith position. 

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

#define MAX 10

  

int n;

vector<int> board(MAX);

  

bool check(int level, int col) {

for(int prev = 0; prev < level; prev++) {

// Conditions when attack can happen

// 1. prev row has the same value as col

// 2. distance between rows is the same as the distance between cols (diagonal)

if(board[prev] == col || abs(level - prev) == abs(col - board[prev])) return false;

}

return true;

}

  

int findMoves(int level) {

if(level == n) { // base case: we have traversed all possible choices

for(int i = 0; i < n; i++) cout << board[i] << " ";

cout << endl;

return 1;

}

int moves = 0;

  

for(int ch = 0; ch < n; ch++) {

if(check(level, ch)) {

board[level] = ch; // place the queen in the valid position

int currMoves = findMoves(level + 1); // find the number of solutions in next levels with the queen placed at the ch col at this level

moves += currMoves;

board[level] = -1; // unplace the queen we just set to check for more choices at the current level

}

}

  

return moves;

}

  

void solve() {

cin >> n;

cout << findMoves(0) << endl;

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