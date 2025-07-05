Source: [[AlgoZenith]]
Topics: [[Bit manipulation]]
Difficulty: [[Easy]]

In this [[DSA]] problem, given an array consisting only of 0s and 1s, find the sum of XOR of every pair in the array.

## Approach -
In problems like these, try to think about it this way.
When, will the sum be incremented? When the output of a XOR is 1.
When will the output of  a XOR be 1? When one bit is 0 and the other is 1.
That creates a relationship.
This is kind of like a combinatorics question. We have to make two choices.
Pick one 0 from the total number of 0s and multiply it with one 1 from the total number of 1s.
That reduces the problem to `totalSum = count0s * count1s`

### Code 
```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

vector<int> vec(n);

  

int count0s = 0, count1s = 0;

  

for(int i = 0; i < n; i++) cin >> vec[i];

for(int i: vec) i ? count1s++ : count0s++;

  

cout << count0s * count1s << endl;

}

  

signed main() {

ios_base::sync_with_stdio(false);

cin.tie(NULL); cout.tie(NULL);

  

int _t;

cin >> _t;

  

while(_t--) solve();

return 0;

}
```

