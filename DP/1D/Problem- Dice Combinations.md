Source: [[CSES]]
Difficulty: [[Easy]]
Topics: [[Dynamic Programming]]

In this [[DSA]] problem, your task is to count the number of ways to construct sum n by throwing a dice one or more times. Each throw produces an outcome between 1 and  6.

For example, if n=3n=3, there are 44 ways:

- 1+1+11+1+1
- 1+21+2
- 2+12+1
- 33

# Input

The only input line has an integer n.

# Output

Print the number of ways modulo 1e9 + 7.

# Constraints

- 1≤n≤10^6

# Example

Input:

3

Output:

4

## Approach 
I tried this in a recursive top down way but it was failing one test case and I couldn't figure out why. So I learned the bottom up way instead which is much much easier.
We want to iterative fill the dp table which represents the current sums.
And for each sum, we want to go from 1 to 6 and find out the number of ways to reach the current sum. Also we take a mod at each step of computation.

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

const int MOD = 1e9 + 7;

  

void solve() {

int sum; cin >> sum;

vector<int> dp(sum + 1);

dp[0] = 1;

  

for(int i = 1; i <= sum; i++) {

int count = 0;

for(int j = 1; j <= 6; j++) {

if(i-j < 0) break;

count = (count + dp[i-j]) % MOD;

}

dp[i] = count;

}

  

cout << dp[sum] << endl;

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