Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given two strings. The task is to find the length of the longest common substring.

##### Input Format

First-line contains T - the number of test cases. Each test case contains two strings s1 and s2 in a single line.

##### Output Format

For each test case, output the length of the longest common substring of the two strings, in a new line.

##### Constraints

1≤T≤1001≤T≤100 1≤∣s1∣≤10001≤∣s1∣≤1000 1≤∣s2∣≤10001≤∣s2∣≤1000 s1s1 and s2s2 contains small letters only.

##### Sample Input 1

3 abc abc algozenith algo algo zenith

##### Sample Output 1

3 4 0

## Approach 
We will solve this using ==form 3== of DP. 
Note: Make sure you don't mistake this as longest common subsequence.
Both problems are very similar. The only difference is in what they do when 2 characters don't match.
I made this mistake and that cost me 2 wrong submissions.

If two characters match, we return 1 + dp(moving both pointers back by one). Else we return 0.
We will start checking for every possible combination of pointers with a nested for loop.

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
string A, B;
int sA, sB;

int rec(int i, int j, vector<vector<int>> &dp) {
    if(i == -1 || j == -1) return 0;

    // cache check
    if(dp[i][j] != -1) return dp[i][j];

    // transition
    int ans = 0;
    if(A[i] == B[j]) ans = 1 + rec(i-1, j-1, dp);

    // save and return
    return dp[i][j] = ans;
}

void solve() {
    cin >> A >> B;
    sA = A.size();
    sB = B.size();
    vector<vector<int>> dp(sA + 1, vector<int> (sB + 1, -1));
    
    int maxi = 0;
    for(int i = sA - 1; i >= 0; i--) {
        for(int j = sB - 1; j >= 0; j--) {
            maxi = max(maxi, rec(i, j, dp));
        }
    }

    cout << maxi << endl;

}

int main() {
    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);
    int _t;
    cin >> _t;
    while(_t--) solve();
    return 0;
}
```