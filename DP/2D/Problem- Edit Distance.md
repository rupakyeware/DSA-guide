Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

##### Input Format

Input contains 2 strings word1 and word2.

##### Output Format

Output the minimum number of operations required to convert word1word1 to word2word2.

##### Constraints

1≤word1.length,word2.length≤5001≤word1.length,word2.length≤500

##### Sample Input 1

horse ros

##### Sample Output 1

3

##### Sample Input 2

intention execution

##### Sample Output 2

5

##### Sample Input 3

catsanddogs ad

##### Sample Output 3

9

## Approach 
This problem is the classical version of [[Problem- Print minimum diff utility string]].
The difference is that here you don't have to print the solution and there is one more operation- replacement.

If the chars of both the strings are the same, we just return the dp of moving both pointers ahead by 1.
Else we return 1 + min of the follows-
1. moving both i and j ahead - replacement 
2. moving only i ahead - deletion
3. moving only j ahead - insertion 
### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

string A, B;
int sA, sB;

int rec(int i, int j, vector<vector<int>> &dp) {
    // base case
    if(i == sA && j == sB) return 0;

    // pruning
    if(i == sA) return sB - j;
    if(j == sB) return sA - i;

    // dp check
    if(dp[i][j] != -1) return dp[i][j];

    // transition
    int ans = 0;
    if(A[i] == B[j]) ans = rec(i+1, j+1, dp);
    else {
        ans = 1 + min({
            rec(i+1, j+1, dp),
            rec(i+1, j, dp),
            rec(i, j+1, dp)
        });
    }

    // save and return
    return dp[i][j] = ans;
}

void solve() {
    cin >> A >> B;
    sA = A.size();
    sB = B.size();
    vector<vector<int>> dp(sA + 1, vector<int> (sB + 1, -1));
    cout << rec(0, 0, dp) << endl;
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