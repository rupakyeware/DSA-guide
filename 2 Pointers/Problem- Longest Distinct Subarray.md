Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]]

In this [[DSA]] problem, you are given an array of N integers. Find the length of the longest subarray with distinct elements.

##### Sample Input 1
```
3
5
1 2 2 1 2
4
3 3 3 3
5
1 3 2 4 1
```

##### Sample Output 1
```
2
1
4
```

## Approach 
We use two pointers, l and r. The r will keep on consuming elements and the l will go on discarding as long as the frq[r] > 1. Then we will update the value of len if needed.

### Code 
```
#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

  

int l = 0;

int len = 0;

unordered_map<int, int> frq;

  

for(int r = 0; r < n; r++) {

frq[vec[r]]++;

  

while(l < r && frq[vec[r]] > 1) {

frq[vec[l]]--;

l++;

}

  

int currLen = r - l + 1;

len = max(currLen, len);

}

  

cout << len << endl;

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