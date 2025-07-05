Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Prefix Sum]], [[Math]]

This is a [[DSA]] problem that I understood 90% of the solution to. But the remaining 10% involves some mathematics that I haven't been able to properly understand yet. 

Given an array of _N_ integers and _Q_ queries. In each query two integers _L_, _R_ is given, you have to find _(A[L] + A[L+1]*2 + A[L+2]*3 + A[L+3]*4...A[R]*(R-L+1))_ % 10^9+7.

## Approach - Using 2 prefix arrays
If we were to make an equation of the result it would be something like
∑A[i] * (i - L + 1), where i = L to R
It can be further expanded as
`∑A[i] * i `+ (1-L) * `∑A[i]`
Now we can see a pattern.
If we turn the first highlight into a prefix array called prefix_b and the second highlight into a prefix array called prefix_a, we can solve this question with 2 prefix arrays.

First we find
`ans = prefix_[r] - prefix_b[l-1]`
after this we get an ans like
`(A[L] * L) + (A[L+1] * L+1...A[R] * R)`
But that's not what we want. 
We want the range of the multiplication part to start from 1 and end with R-L+1.

Here comes the part I didn't really understand
we do
`ans -= (l-1) * (prefix_a[r] - prefix_a[l-1]);`
This converts the ans into the format we wanted. 
What I didn't really understand was **how** that happened.

### Code

```
#include "../stdc++.h"

#define int long long

#define endl '\n'

const int MOD = 1e9 + 7;

using namespace std;

  

void solve() {

int n, q; cin >> n >> q;

vector<int> v(n);

for(int i = 0; i < n; i++) cin >> v[i];

  

// Prefix sum of v[i]

vector<int> prefix_a(n+1, 0);

for(int i = 1; i <= n; i++) {

prefix_a[i] = ((prefix_a[i-1] % MOD) + (v[i-1]) % MOD) % MOD;

}

  

// Prefix sum of v[i] * i;

vector<int> prefix_b(n+1, 0);

for(int i = 1; i <= n; i++) {

prefix_b[i] = ((prefix_b[i-1] % MOD) + (v[i-1] * i) % MOD) % MOD;

}

  

while(q--) {

int l, r; cin >> l >> r;

int ans = prefix_b[r] - prefix_b[l-1]; // This will give us (v[l] * l) + (v[l] * (l+1)) etc.

ans -= (l-1) * (prefix_a[r] - prefix_a[l-1]); // Then after some calculation that I didn't really understand much we get the correct answer

ans = (ans % MOD + MOD) % MOD;

cout << ans << endl;

}

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