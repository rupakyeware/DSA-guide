Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Bit manipulation]]

In this [[DSA]] problem, given an array where every element occurs twice, except two elements which occur only once. Find both the elements which occur only once.

**Example:**

1. {23, 23, 4, 3, 5, 3, 15, 15} => 4,5
2. {15, 10, 12, 10, 19, 19} => 12,15

## Approach 
This was a tricky question. But it became simple once I understood how it worked. 
Essentially, we want to approach it like [[Problem- Find element occurring odd number of times]]. But for that we'd need only one element to appear once. Here, we have 2. So we will divide the array into 2 where each array will contain only one of those 2 elements. 

But how can we divide them?

First, we'll find the xor of the entire array. If a and b are the two elements occurring once, the xor will contain a ^ b. Now we want to get the lsb of this xor. We can do that with `xor & ~(xor-1)`. Now we know which was the least significant bit that made the xor of a and b to result in a set bit. So we can divide the array based on this bit. One array that contains all the elements having this bit as set, and another containing all the elements having this bit as unset. Now we can easily find the individual elements by just xoring the array once again.

### Code 
```cpp
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

vector<int> vec(n);

for(int i = 0;i < n; i++) cin >> vec[i];

  

// Get the xor of the entire array

int XOR = 0;

for(int i: vec) XOR ^= i;

  

int lsb = XOR & ~(XOR-1);

  

vector<int> s1; // array containing set lsb

vector<int> s2; // array containing unset lsb

  

for(int i: vec) {

if(lsb & i) s1.push_back(i);

else s2.push_back(i);

}

  

// Find 1st ans

XOR = 0;

for(int i: s1) XOR ^= i;

cout << XOR << " ";

  

// Find 2nd ans

XOR = 0;

for(int i: s2) XOR ^= i;

cout << XOR << endl;

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