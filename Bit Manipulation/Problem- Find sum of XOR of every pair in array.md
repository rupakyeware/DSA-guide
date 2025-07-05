Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Bit manipulation]]

In this [[DSA]] problem, you are given an array of elements of size n. Return the sum of XORs of every pair in the array.

## Approach 
I had trouble understanding this approach when it was first taught in the class. But I went through the lecture again and was able to code it myself after I properly understood the solution.

Essentially, we have to break it down into [[Problem- Find sum of XOR of every pair in 01 array]].
The way we do that is by approaching every element bitwise.
So first we'll find the XOR of all 0s and 1s at the 0th bit of every element, then the 1st bit, then the 2nd, and so on until we reach the last bit of the largest number in the array.
It's actually pretty simple really, just needs a bit of time to properly fit into your mind.

The way I've written the code should also make it pretty to understand and revise.

### Code 
```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n; cin >> n;

vector<int> v(n);

int max_v = INT_MIN; // Keep a track of the largest number in the array

for(int i = 0; i < n; i++) {

cin >> v[i];

max_v = max(max_v, v[i]);

}

  

// Find no. of bits in max_v

int limit = 0;

while(max_v) {

limit++;

max_v = max_v >> 1;

}

  

int ans = 0; // Sum of XORs of all pairs

int multiplier = 1; // Power of 2 at each level (2^0, 2^1, etc)

// Another way to find the power easily would be to use (1 << i).

for(int i = 0; i < limit; i++) {

int count1s = 0; // Keep a track of the no. of 1s at current level

  

// Find the no. of 1s at current level

for(int j = 0; j < n; j++) {

int num = v[j];

count1s += 1 & (num >> i);

}

  

int count0s = n - count1s; // Find no. of 0s at current level from count1s

int currAns = multiplier * (count0s * count1s); // Ans at current level

ans += currAns; // Update ans

multiplier *= 2; // Update multiplier (x2)

}

  

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