Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]]

In this [[DSA]] problem, given an array of _N_ integers, find the length of the smallest sub-array that contains all the distinct elements of the array.

##### Sample Input 1
```
6
5
1 1 3 2 3
5
1 2 3 4 5
6
1 2 2 3 3 4
6
1 2 1 3 2 4
5
1 1 1 1 1
1
1
```

##### Sample Output 1
```
3
5
6
4
1
1
```

## Approach 
Here too, we will be using 2 pointers- l and r. The r will go on consuming elements. The l will go on removing elements while the size of the frq map is >= number of distinct elements. Every time l finds a valid subarr, it will update the shortest variable if needed

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

  

// Count the number of distinct elements in the array

unordered_set<int> s;

for(int i: vec) s.insert(i);

int size = s.size();

  

int l = 0, shortest = INT_MAX;

unordered_map<int, int> frq;

for(int r = 0; r < n; r++) {

frq[vec[r]]++;

while(frq.size() >= size) {

int currLen = r - l + 1;

shortest = min(shortest, currLen);

if(frq[vec[l]] == 1) frq.erase(vec[l]);

else frq[vec[l]]--;

l++;

}

}

  

cout << shortest << endl;

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