In this [[DSA]] problem, you are given a stick of length `n` and a list of integers `cuts`, where each cut represents a position to cut the stick. After a cut, you have two smaller sticks. You need to perform all the cuts in any order.

The **cost** of a single cut is the length of the stick being cut at that moment.  
Return the **minimum total cost** to perform all the cuts.

---

### 🔢 Example:

**Input:**

`n = 9 cuts = [5,6,1,4,2]`

**Output:**

`22`

## Approach-
We will use ==form 4== of dp here which is interval dp or LR dp.
The cost of cutting a rod from L to R is as follows
`cost(l, r) = len(l, r) + cost(l, m) + cost(m, r)`
we will use dp to break the recursive cost function down and solve this problem optimally

### Code 
```cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

int rec(vector<int> &arr, int L, int R, vector<vector<int>> &dp) {
    if(dp[L][R] != -1) return dp[L][R];

    int dist = arr[R] - arr[L];
    int mini = INT_MAX;
    for(int i = L + 1; i < R; i++) {
        mini = min(mini, dist + rec(arr, L, i, dp) + rec(arr, i, R, dp));
    }

    return (mini == INT_MAX ? 0 : dp[L][R] = mini);
}

void solve() {
    int n; cin >> n;
    vector<int> arr(n);
    arr.insert(arr.begin(), 0);
    for(int i = 1; i <= n; i++) cin >> arr[i];
    
    vector<vector<int>> dp(n + 3, vector<int> (n+2 , -1));

    cout << rec(arr, 0, n, dp) << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    while(_t--) solve();
    return 0;
}
```
