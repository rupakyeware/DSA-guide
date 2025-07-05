Source: [[CodeForces]]
Difficulty: [[800]]
Topics: [[Greedy]], [[Math]]

In this [[DSA]] problem, you are given an array consisting of n integers. Your task is to determine whether it is possible to colour all its elements in two colours in such a way that the sums of the elements of both colours have the same parity and each colour has at least one element coloured.

For example, if the array is [1,2,4,3,2,3,5,41,2,4,3,2,3,5,4], we can colour it as follows: [1,2,4,3,2,3,5,41,2,4,3,2,3,5,4], where the sum of the blue elements is 66 and the sum of the red elements is 1818.

### Approach
Once I saw the pattern, this was a very easy problem. 
Even + Even = Even
Odd + Odd = Even
Odd + Even = Odd
We can see that as long as the number of odd numbers are even, the output will always be an even number. And that's all we have to check. If the count of the odd numbers in the given array is even. If it is, return true. Else return false.

### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

bool isPossibleToColour(int o) {

    if(o % 2 == 0) return true;

    else return false;

}

  

void solve() {

    int n; cin >> n;

    int o = 0;

    for(int i = 0; i < n; i++) {

        int x; cin >> x;

        if(x % 2) o++;

    }

  

    cout << (isPossibleToColour(o) ? "YES" : "NO") << endl;

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