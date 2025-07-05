Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]]

In this [[DSA]] problem, you are given an array of N integers and an integer D. Consider all subarrays with length D, the penalty of the subarray is the number of distinct elements present in the subarray. Find a subarray of length D with minimum penalty. Print the minimum penalty.

##### Sample Input 1
```
5
6 3
0 1 1 2 2 2
5 3
1 0 1 2 3
5 5
1 1 2 3 4
5 1
1 2 3 4 5
7 3
1 2 1 2 3 4 2
```

##### Sample Output 1
```
1
2
4
1
2
```

## Approach 
This was a pretty simple problem. We need to first calculate the number of distinct elements in the first window, which will be the initial value of penalty.
Then we just go on moving the window ahead, if we encounter an element with a frq of 0, we increment the count of distinct elements. Similarly, if we are discarding an element with a count of 1, we decrement the count of distinct elements. Finally, we check if the count of distinct elements is less than the value of penalty. If so, we update penalty.

### Code 
```
#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n, d; cin >> n >> d;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

  

int penalty = 0;

int distElemsInWindow = 0;

unordered_map<int, int> frq;

  

// Get penalty of initial window

for(int i = 0; i < d; i++) {

if(frq[vec[i]] == 0) distElemsInWindow++;

frq[vec[i]]++;

}

penalty = distElemsInWindow;

// Get minimum penalty of all windows of size d

int l = 0;

for(int r = d; r < n; r++) {

// Consume rth element

if(frq[vec[r]] == 0) distElemsInWindow++;

frq[vec[r]]++;

  

// Discard lth element

if(frq[vec[l]] == 1) distElemsInWindow--;

frq[vec[l]]--;

l++;

  

// Update penalty

penalty = min(penalty, distElemsInWindow);

}

  

cout << penalty << endl;

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