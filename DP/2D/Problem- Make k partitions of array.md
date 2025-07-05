Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are given an array `nums` of length `n` and an integer `k`. You must split the array into exactly `k` **non-empty contiguous subarrays**.

For each subarray, determine its minimum element. Your goal is to maximize the **sum of these minimum elements**across all `k` subarrays.

Return the **maximum sum** possible.

---

### 🔶 Example 1:

**Input:**

`nums = [1, 3, 5, 2, 9] k = 3`

**Output:**

`12`

**Explanation:**  
Split the array as `[1, 3, 5]`, `[2]`, `[9]`.  
Minimums are `1`, `2`, and `9`, respectively.  
Sum = 1 + 2 + 9 = **12**

---

### 🔶 Example 2:

**Input:**


`nums = [4, 2, 7, 6, 5] k = 2`

**Output:**


`9`

**Explanation:**  
Split as `[4, 2, 7]` and `[6, 5]`.  
Minimums are `2` and `5`.  
Sum = 2 + 5 = **7`**, but better is` [4, 2]`and`[7, 6, 5]`→ 2 + 5 = **7**. Actually, optimal split is`[4]`and`[2, 7, 6, 5]`→ 4 + 2 = **6**, which is less. → Best is`[4, 2, 7]`and`[6, 5]` → 2 + 5 = **7**

Oops! But you can actually do `[4, 2]` and `[7, 6, 5]` → 2 + 5 = **7**.  
→ **Correction**: Optimal sum is **7**

(Note: Adjust this example to get a clearer optimal one.)

---

### 🔶 Example 3:

**Input:**


`nums = [8, 1, 9, 2, 7] k = 2`

**Output:**


`9`

**Explanation:**  
Split as `[8, 1, 9]` and `[2, 7]` → mins: 1 + 2 = 3  
Try `[8]` and `[1, 9, 2, 7]` → mins: 8 + 1 = 9 ← best

---

### 🔸 Constraints:

- `1 <= nums.length <= 1000`
    
- `1 <= k <= nums.length`
    
- `1 <= nums[i] <= 10^4`

## Approach-
This problem can be solved using ==form 2== of [[Dynamic Programming]].
So given an array, we need to find the best place to make the right most partition and then recursively find the remaining partitions. 
The transition formula will be-

`dp(i, x) -> dp(j, x-1) + min(j+1...i)`
where,
i is the index of the element to include in the next partition 
x is the amount of partitions to make 
j is where the new partition will start from 
min(j+1...i) is the minimum element in the partition we just made 

### Code 
``` cpp
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

/*
    Function to recursively compute the maximum sum of minimums of `x` segments
    in the subarray nums[0..idx].
*/
int rec(vector<int> &nums, int idx, int x, vector<vector<int>> &dp) {
    // Not enough elements to create x partitions
    if (idx + 1 < x) return INT_MIN;

    // Base case: if we've used all elements and all partitions
    if (idx == -1) return (x == 0 ? 0 : INT_MIN);

    // DP memoization: shifted by 1 to handle idx = -1
    if (dp[idx + 1][x] != -1) return dp[idx + 1][x];

    int ans = INT_MIN;
    int mini = INT_MAX;

    // Try all possible partitions ending at `idx`
    for (int j = idx - 1; j >= -1; j--) {
        mini = min(mini, nums[j + 1]);  // minimum of current segment
        int prev = rec(nums, j, x - 1, dp);
        if (prev != INT_MIN)
            ans = max(ans, prev + mini);
    }

    return dp[idx + 1][x] = ans;
}

void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> nums(n);
    for (int i = 0; i < n; i++) cin >> nums[i];

    // Shifted index by 1 to accommodate idx = -1
    vector<vector<int>> dp(n + 2, vector<int>(k + 1, -1));

    cout << rec(nums, n - 1, k, dp) << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    // cin >> _t;
    while (_t--) solve();
    return 0;
}

```