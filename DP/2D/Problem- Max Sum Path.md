Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are given a 2D grid of integers `grid` with `n` rows and `m` columns. Your task is to find a path from the **top-left corner**`(0, 0)` to the **bottom-right corner** `(n - 1, m - 1)` that **maximizes the sum of values along the path**.

At each step, you can only move **right** or **down**.

Return the **maximum path sum** you can achieve.

---

### **Example 1**

**Input:**

csharp

CopyEdit

`grid = [   [1, 3, 5, 7],   [-3, 2, 6, 9],   [4, 0, 8, -2],   [3, 2, 11, 4] ]`

**Output:** `38`

**Explanation:**

One optimal path is:

CopyEdit

`1 → 3 → 5 → 7 → 9 → -2 → 4`

Coordinates:  
(0,0) → (0,1) → (0,2) → (0,3) → (1,3) → (2,3) → (3,3)  
Sum = 1 + 3 + 5 + 7 + 9 + (-2) + 4 = **27** — not optimal.

Better path:  
(0,0) → (0,1) → (1,1) → (1,2) → (2,2) → (3,2) → (3,3)  
Sum = 1 + 3 + 2 + 6 + 8 + 11 + 4 = **35**

But best path is:  
(0,0) → (0,1) → (1,1) → (1,2) → (1,3) → (2,3) → (3,3)  
Sum = 1 + 3 + 2 + 6 + 9 + (-2) + 4 = **23** — still worse

Actual best:  
(0,0) → (0,1) → (1,1) → (1,2) → (2,2) → (3,2) → (3,3)  
Sum = 1 + 3 + 2 + 6 + 8 + 11 + 4 = **35**

Best sum found via correct DP: **38**

---

### **Constraints**

- `1 <= n, m <= 1000`
    
- `-10^4 <= grid[i][j] <= 10^4`

## Approach 
This problem was pretty simple to solve. It can be solved with ==form 2==.
We start from the last cell (n-1, n-1 if it's 0 indexed which is how I've done it) and then try to calculate the max values we can get if we reached here from above or from the left and that value will be the value of this particular cell. 
So the transition will be like 
`dp(r,c) = max(dp(r, c-1) + arr[r][c], dp(r-1, c) + arr[r][c])`

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

int n, m; 

int rec(vector<vector<int>> &arr, int r, int c, vector<vector<int>> &dp) {
    // pruning 
    if(r < 0 || r >= n || c < 0 || c >= m) return INT_MIN;

    // base case 
    if(r == 0 && c == 0) return 0 + arr[r][c]; // 0 + value of the first cell

    // dp check 
    if(dp[r][c] != INT_MIN) return dp[r][c];

    // transition 
    int ans = max(rec(arr, r, c-1, dp) + arr[r][c], rec(arr, r-1, c, dp) + arr[r][c]); // max of (coming from left, coming from above)

    // save and return 
    return dp[r][c] = ans;
}

void solve() {
    cin >> n >> m;
    vector<vector<int>> arr(n, vector<int>(m, 0));
    vector<vector<int>> dp(n+1, vector<int> (m+1, INT_MIN));
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) 
            cin >> arr[i][j];
    }

    cout << rec(arr, n-1, m-1, dp) << endl;
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

