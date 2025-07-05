Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]]

In this [[DSA]] problem, given an array _A_ of _N_ integers and an integer _target_, find three integers in _A_ such that the sum is closest to the _target (absolute value of (sum-target) is minimum)_. Print the minimum absolute value of (sum-target). You cannot select an index more than one. All three indexes should be distinct.

The first line contains _T_, the number of test cases _(1<=T<=100)_.
The first line contains two space-separated integers _N, target_ where 3_<=N<=10^3, -1e18<=target<=1e18._
Next line contains _N_ space-separated integers (-1e9<=Ai<=1e9).
The Sum of _N_ across all test cases ≤ 10^4.

For each test case print the minimum absolute value of (sum-target).

Sample Input
```
1
5 3
1 2 3 4 5
```

Sample Output
```
3
```

## Approach
The question does not state anywhere that the given array will be sorted.
But it also doesn't state that we CAN'T sort it.
So sort it, we will.

Then, we iterate the first pointer from 0 to n - 2
On the remaining window, we will perform a basic 2 Sum. Where if the currSum > sum, we decrement the right pointer, else increment the left pointer.

Then at every step, we will calculate the current abs value. And if it's lower than the minAbs value, we will update minAbs.

### Code 
```
#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n, target; cin >> n >> target;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

sort(vec.begin(), vec.end());

  

int minAbs = LLONG_MAX;

for(int p1 = 0; p1 < n - 2; p1++) {

int p2 = p1 + 1;

int p3 = n - 1;

while(p2 < p3) {

int currSum = vec[p1] + vec[p2] + vec[p3];

int currDiff = llabs(currSum - target);

minAbs = min(currDiff, minAbs);

if(currSum < target) p2++;

else if(currSum > target) p3--;

else break;

}

}

  

cout << minAbs << endl;

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