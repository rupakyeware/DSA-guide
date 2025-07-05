Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

This is a variation of [[Problem- 01 Knapsack]] where instead of either choosing to either include the item 0 or 1 time in the knapsack, we can include it as many times as we want.

## Approach-
A very minor change is needed. The process for not picking the item will remain the same. But when we decide to pick an item, instead of going to a new state, we just stay in the same state. 

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

vector<int> solution;

  

int dfs(vector<vector<int>> &dp, vector<int> &wt, vector<int> &val, int W, int level, int remainingCap, int n) {

if(level == n || remainingCap == 0) return 0;

if(dp[level][remainingCap] != -1) return dp[level][remainingCap];

  

// don't pick current item

int noPick = dfs(dp, wt, val, W, level + 1, remainingCap, n);

  

// if remaining capacity allows picking the current item, pick it

int pick = 0;

if(wt[level] <= remainingCap) {

pick += val[level] + dfs(dp, wt, val, W, level, remainingCap - wt[level], n); // val of currItem + dfs of next item

}

  

return dp[level][remainingCap] = max(noPick, pick);

}

  

void solve() {

int n; cin >> n;

int W; cin >> W;

  

vector<int> wt(n);

vector<int> val(n);

for(int i = 0; i < n; i++) cin >> wt[i]; //accept weights of all items

for(int i = 0; i < n; i++) cin >> val[i]; // accept values of all items

  

vector<vector<int>> dp(n + 1, vector<int> (W + 1, -1)); // initialize 2D dp table

  

cout << dfs(dp, wt, val, W, 0, W, n) << endl;

  

cout << endl;

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