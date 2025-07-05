Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Bit manipulation]]

In this [[DSA]] problem, given an array where every element occurs three times, except one element which occurs only once. Find the element that occurs once.

**Example:**

1. {23, 5, 23, 4, 23, 4, 5, 3, 5, 4} => 3
2. {15, 12, 15, 9, 15, 9, 9} => 12

The solution to this problem using hash maps is very easy and obvious. But that also taken O(n) memory space. This approach using bit manipulation takes O(1) memory.
## Approach 1- Bit Manipulation 
This is a very interesting approach. We want to iterate through all the elements in a bitwise manner while calculating the sum of the values of their bits at that position.
If the sum isn't divisible by 3, it means the bit in the position for the answer is set in that position. Just reading won't be enough to understand the solution. It's better to write it out.

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

// Loop through each bit

for(int i = 0; i < 32; i++) {

int sum = 0;

  

// Loop through each element

for(int j = 0; j < n; j++) sum += vec[j] & (1 << i); // Add the val of the 2^ith bit to the sum

  

// If sum isn't divisble by 3, the ith bit is set in the unique element

if(sum % 3) ans = ans | (1 << i);

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
