Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are given n items and their weights along with their values. You are also given a max capacity W.
You have to find the maximum possible value that can be obtained by choosing the elements such that the weight of the items in the knapsack doesn't exceed W.

## Approach 1- Recursion with memoization
Here, we will recursively either pick or not pick the nth item and then store the max value that can be obtained from both the choices in the dp table such that if we encounter the same number of items again with a same remaining capacity, we can just look up the value from the table instead of calling all the functions again.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

int dfs(vector<vector<int>> &dp, vector<int> &wt, vector<int> &val, int W, int noItems, int currCapacity) {

if(noItems == 0 || currCapacity == 0) return 0;

if(dp[noItems][currCapacity] != -1) return dp[noItems][currCapacity];

  

// don't pick current item

int noPick = dfs(dp, wt, val, W, noItems - 1, currCapacity);

  

// if remaining capacity allows picking the current item, pick it

int pick = 0;

if(wt[noItems - 1] <= currCapacity) {

pick += val[noItems - 1] + dfs(dp, wt, val, W, noItems - 1, currCapacity - wt[noItems - 1]); // val of currItem + dfs of next item

}

  

return dp[noItems][currCapacity] = max(noPick, pick);

  

}

  

void solve() {

int n; cin >> n;

int W; cin >> W;

  

vector<int> wt(n);

vector<int> val(n);

for(int i = 0; i < n; i++) cin >> wt[i]; //accept weights of all items

for(int i = 0; i < n; i++) cin >> val[i]; // accept values of all items

  

vector<vector<int>> dp(n + 1, vector<int> (W + 1, -1)); // initialize 2D dp table

  

cout << dfs(dp, wt, val, W, n, W) << endl;

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

## Approach 2- Bottom up
Here, we use the logic we learned in design and analysis of algorithms (DAA) where we fill the table starting from the 0th item at the 0th weight of knapsack up to the last item until the highest capacity of the knapsack.

We are still using the logic of taking the max value if we pick or don't pick the current item, just doing it bottom up.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

int W; cin >> W;

  

vector<int> wt(n);

vector<int> val(n);

for(int i = 0; i < n; i++) cin >> wt[i]; //accept weights of all items

for(int i = 0; i < n; i++) cin >> val[i]; // accept values of all items

  

vector<vector<int>> dp(n + 1, vector<int> (W + 1, 0)); // initialize 2D dp table

  

for(int i = 1; i <= n; i++) { // iterate through all items

for(int w = 1; w <= W; w++) { // iterate through all possible weights of i-1th item

if(wt[i-1] <= w) { // if weight of current item is less than wth capacity of knapsack

dp[i][w] = max( // max of previous value (don't pick current item) and value if we pick the current item

dp[i-1][w],

val[i-1] + dp[i-1][w-wt[i-1]]

);

}

else { // just copy what we had done previously (without the current item)

dp[i][w] = dp[i-1][w];

}

}

}

  

cout << dp[n][W] << endl;

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

# Variation 1- Print solution 
In this variation, along with the final value of the answer, also print the indexes of the chosen elements.

## Approach
In this approach, I've changed the logic according to Vivek sir's solution. The only difference in the previous logic and his logic is that he starts from the left in picking or not picking the element whereas in the previous approach, we started making the choice from the right.

To print the solution, we basically make a new function which is almost an exact copy of the dfs function except here, we won't have to compute anything again as everything will already be stored in the dp array.

So when we call display(0, W), we call dfs(1, W) and dfs(1, W-wt[level]) and get the answer in O(1) so we can make the choice much quicker this time.
If we choose to pick, we append the level we are at to a solution vector and then go to the next level.

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

pick += val[level] + dfs(dp, wt, val, W, level + 1, remainingCap - wt[level], n); // val of currItem + dfs of next item

}

  

return dp[level][remainingCap] = max(noPick, pick);

  

}

  

void display(vector<vector<int>> &dp, vector<int> &wt, vector<int> &val, int W, int level, int remainingCap, int n) {

if(level == n || remainingCap == 0) return;

int noPick = dfs(dp, wt, val, W, level, remainingCap, n);

int pick = 0;

if(wt[level] <= remainingCap) {

pick += val[level] + dfs(dp, wt, val, W, level + 1, remainingCap - wt[level], n);

}

  

if(noPick > pick) {

display(dp, wt, val, W, level + 1, remainingCap, n);

}

else {

solution.push_back(level);

display(dp, wt, val, W, level + 1, remainingCap - wt[level], n);

}

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

  

// this function is very similar to dfs but since the required values are already stored in dp, they will be retrieved in O(1)

display(dp, wt, val, W, 0, W, n);

for(int i: solution) cout << i << " ";

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

# Variation 2- Pick at max K items 
In this variation, an extra constraint is add where you can pick at max K items in the knapsack.

## Approach
This is a pretty easy modification where we just have to add an extra dimension to the dp array to compensate for the extra constraint and add an extra argument to the dfs function for the same.

### Code
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

int dfs(vector<vector<vector<int>>> &dp, vector<int> &wt, vector<int> &val, int W, int noItems, int currCapacity, int itemsLeft) {

if(noItems == 0 || currCapacity == 0 || itemsLeft == 0) return 0;

if(dp[noItems][currCapacity][itemsLeft] != -1) return dp[noItems][currCapacity][itemsLeft];

  

// don't pick current item

int noPick = dfs(dp, wt, val, W, noItems - 1, currCapacity, itemsLeft);

  

// if remaining capacity allows picking the current item, pick it and reduce the number of items left

int pick = 0;

if(wt[noItems - 1] <= currCapacity) {

pick += val[noItems - 1] + dfs(dp, wt, val, W, noItems - 1, currCapacity - wt[noItems - 1], itemsLeft-1); // val of currItem + dfs of next item

}

  

return dp[noItems][currCapacity][itemsLeft] = max(noPick, pick);

  

}

  

void solve() {

int n; cin >> n;

int W; cin >> W;

int K; cin >> K;

  

vector<int> wt(n);

vector<int> val(n);

for(int i = 0; i < n; i++) cin >> wt[i]; //accept weights of all items

for(int i = 0; i < n; i++) cin >> val[i]; // accept values of all items

  

vector<vector<vector<int>>> dp(n + 1, vector<vector<int>> (W + 1, vector<int> (K + 1, -1)));

cout << dfs(dp, wt, val, W, n, W, K) << endl;

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