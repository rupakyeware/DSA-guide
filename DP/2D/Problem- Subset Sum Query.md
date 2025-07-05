Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Dynamic Programming]], [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given an array of size NN, and QQ queries, for each query, you need to get the indices of the elements of the array whose subset-sum is equal to the queried sum​, if possible, else return −1−1.

Complete the Function **subset_queries( vector &arr, vector &queries )** that takes vector aa and queries vector as input.

##### Input Format

The first line of input contains two integers - NN, QQ where NN is the size of the array and QQ is a number of queries. The second line of input contains NN space-separated integers, which are array elements. The third line of input contains QQ space-separated integers, which are queries.

##### Output Format

Return a **vector < vector < int > >** having 00-based indices of the elements of the array whose subset-sum is equal to the queried sum i​ for each ithi query, if possible, else return vector { −1−1 }. If the returned **vector < vector < int > >** from the function **subset_queries( vector &arr, vector &queries )** is valid, then the program prints 1. Otherwise, prints -1.

##### Constraints

1≤N≤1001≤N≤100 , size of **vector < int > arr** 1≤Q≤1051≤Q≤105 , size of **vector < int > queries** 1≤arr[i]≤1051≤arr[i]≤105 1≤sumi≤1051≤sumi​≤105

##### Sample Input 1

5 3 1 2 3 4 5 7 16 3

##### Sample Output 1

1 -1 1

## Approach 
This might be the hardest question I've done yet in [[Dynamic Programming]]. I had to see Vivek Sir's video for normal subset sum, try to code it for this problem, fail, look at the solution code, not understand it, query chatgpt to explain it to me, not understand it, then try to do it myself, fail, query chatgpt again, and then finally understand it. 

But this question was fun.

So essentially, we start from the right and go towards the left while asking can the remaining sum of x be achieved with elements from 0 to i?
To put it into dp terms-
`dp(i, remainingSum) = dp(i-1, remainingSum) || dp(i-1, remainingSum-arr[i])`
This means, we are either picking the current element or not picking it.

My first approach was to go from left to right and make a new dp table each time. But that got a TLE since my approach was basically to see if a final sum of x can be achieved with elements from 0 to i. And while the approach itself wasn't wrong, it wasn't efficient enough. 

The efficient approach was to flip the direction so that we can reuse the dp table. I got to learn a lot from this question. Need to do more such questions.

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include<bits/stdc++.h>
using namespace std;
#define MAX 1e5

#define ll int64_t


int dfs(vector<int> &arr, vector<vector<int>> &dp, vector<int> &queries, int query_idx, int idx, int remainingSum) {
    // pruning
    if(remainingSum < 0) return 0;

    // base case
    if(idx == -1) {
        if(remainingSum == 0) return 1;
        else return 0;
    }

    // cache check
    if(dp[idx][remainingSum] != -1) return dp[idx][remainingSum];

    // transitions if we don't pick or pick current idx
    int ans = dfs(arr, dp, queries, query_idx, idx - 1, remainingSum) || dfs(arr, dp, queries, query_idx, idx - 1, remainingSum - arr[idx]);

    // save and return
    return dp[idx][remainingSum] = ans;

}

vector<vector<int>> subset_queries(vector<int> &arr, vector<int> &queries) {
    int n = arr.size();
    vector<vector<int>> ans;
    vector<vector<int>> dp(n + 1, vector<int> (1e5 + 1, -1));

    for(int i = 0; i < queries.size(); i++) {
        vector<int> currAns;
        int possible = dfs(arr, dp, queries, i, n - 1, queries[i]);
        if(possible) {
            int remainingSum = queries[i], idx = n-1;
            while(remainingSum > 0 && idx >= 0) {
                if(dfs(arr, dp, queries, queries[i], idx - 1, remainingSum - arr[idx])) {
                    currAns.push_back(idx);
                    remainingSum -= arr[idx];
                }
                idx--;
            }
            reverse(currAns.begin(), currAns.end());
            ans.push_back(currAns);
        }
        else ans.push_back({-1});
    }

    return ans;
}


void solve() {
    int N, Q;
    cin >> N >> Q;
    vector<int> arr(N);
    for (int i = 0; i < N; i++)cin >> arr[i];
    vector<int> queries(Q);
    for (int i = 0; i < Q; i++)cin >> queries[i];
    auto ans = subset_queries(arr, queries);

    // checker.
    if (ans.size() != Q) {
        cout << 101 << endl;
        return;
    }
    for (int i = 0; i < Q; i++) {
        auto x = ans[i];
        if (x.size() == 0) {
            cout << 101 << endl;
            continue;
        }
        if (x.size() == 1 && x[0] == -1) {
            cout << -1 << endl;
            continue;
        }
        ll sum = 0, p = -10;
        for (auto y : x) {
            if (y < 0 || y >= N || p >= y ) { // valid 0-indexed.
                sum = -1111;
                break;
            }
            p = y;
            sum += arr[y];
        }
        if (sum == queries[i]) {
            cout << 1 << endl;
        }
        else cout << 101 << endl;
    }
}
int main() {
    ios_base :: sync_with_stdio(0);
    cin.tie(nullptr); cout.tie(nullptr);

#ifdef Mastermind_
    freopen("input.txt", "r", stdin); \
    freopen("output.txt", "w", stdout);
#endif
    int t = 1;
    // int i = 1;
    // cin >> t;
    while (t--) {
        // cout << "Case #" << i << ": ";
        solve();
        // i++;
    }
    return 0;
}
```