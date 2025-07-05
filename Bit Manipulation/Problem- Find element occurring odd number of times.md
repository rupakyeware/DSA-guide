Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Bit manipulation]]

Given an array where every element occurs an even number of times, except one element which occurs an odd number of times. Find the element that occurs an odd number of times.

**Example:**

1. {23, 15, 23, 4, 23, 4, 15, 4, 23, 15, 15} => 4
2. {15, 19, 12, 15, 19, 19, 19} => 12

This [[DSA]] problem can easily be solved using an [[Unordered Map]] but that will need O(n) memory and a 2 pass. We'll do it in a single pass and O(1) memory using [[Bit manipulation]].

## Approach 
The bitwise operation we'll be using here is XOR. That is because A ^ A = 0. So the even elements will be cancelled out. And also, XOR is not order dependent. 
So we will iterate through the array while taking XOR. In the end, only the element occurring an odd number of times will be left in the ans.

### Code 
```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

  

int ans = 0;

for(int i = 0; i < n; i++) ans = ans ^ vec[i];

cout << ans << endl;

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
