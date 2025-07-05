Source: [[Hackerrank]]
Difficulty: [[Easy]]
Topics: [[String]]

In this [[DSA]] problem, you are given a string s, and the amount of left shifts and right shifts to be performed on it. Return the final output after performing the left shifts and right shifts.

I got this problem in [[Nvidia]]'s OA on [[Hackerrank]]. And I'm extremely embarrassed  to say I could only think of the bruteforce approach which failed 7/11 test cases.

I am not going to let myself drown in embarrassment though. Sure I feel it. But I have to bounce back. Here's the approach for it.

## Approach-
If the value of leftShift or rightShift > len of string, we can mod it with the length of the string. 
Also, a right shift can be converted into a left shift by subtracting its value from the total length of the string.

### Code 
```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

string shiftString(string s, int leftShift, int rightShift) {

int len = s.size();

leftShift %= len;

rightShift %= len;

  

// Perform left shift

s = s.substr(leftShift) + s.substr(0, leftShift);

// Perform right shift

int equi_leftShift = len - rightShift;

s = s.substr(equi_leftShift) + s.substr(0, equi_leftShift);

  

return s;

}

  

void solve() {

string s;

int leftShift, rightShift;

cin >> s >> leftShift >> rightShift;

cout << shiftString(s, leftShift, rightShift) << endl;

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