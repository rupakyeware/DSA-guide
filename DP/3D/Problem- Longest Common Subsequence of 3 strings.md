Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given 3 strings s1, s2 and s3, the task is to find the length of the longest common sub-sequence in all three given strings.

##### Input Format

First-line contains T - the number of test cases. Each test case contains 3 strings in a single line.

##### Output Format

For each test case, print the length of the longest common subsequence in all the 3 given strings, in a new line.

##### Constraints

1≤T≤1001≤T≤100 1≤∣s1∣≤1001≤∣s1∣≤100 1≤∣s2∣≤1001≤∣s2∣≤100 1≤∣s3∣≤1001≤∣s3∣≤100

##### Sample Input 1

3 abc abc bbc algozenith algo algorithm algo zenith zen

##### Sample Output 1

2 4 0

## Approach-
Using form 2 of dp, we can solve this quite easily. We place the pointers at the end of the 3 strings and then move them as follows-
1. if all 3 chars are the same, return dp(a-1, b-1, c-1) + 1
2. else return the max of moving each character back by one in 3 different func calls.

And we use dp to memoize the solution 

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

string A, B, C;
int sizeA, sizeB, sizeC;

int check(int a, int b, int c, vector<vector<vector<int>>> &dp) {
    // pruning
    if(a < 0 || a >= sizeA) return 0;
    if(b < 0 || b >= sizeB) return 0;
    if(c < 0 || c >= sizeC) return 0;

    // basecase 

    // dp check
    if(dp[a][b][c] != -1) return dp[a][b][c];

    // transition
    int ans = 0;
    if(A[a] == B[b] && B[b] == C[c]) ans = check(a-1, b-1, c-1, dp) + 1;
    else {
        ans = max({
            check(a-1, b, c, dp),
            check(a, b-1, c, dp),
            check(a, b, c-1, dp)
        });
    }

    // save and return
    return dp[a][b][c] = ans;
}

void solve() {
    cin >> A >> B >> C;
    sizeA = A.size(); sizeB = B.size(); sizeC = C.size();
    vector<vector<vector<int>>> dp(sizeA + 1, vector<vector<int>> (sizeB + 1, vector<int> (sizeC + 1, -1)));

    cout << check(sizeA-1, sizeB-1, sizeC-1, dp) << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    cin >> _t;

    while(_t--) solve();
    return 0;
}
```