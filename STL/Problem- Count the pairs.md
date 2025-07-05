Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[lower_bound]]

In this [[DSA]] question you are given an array A of size N. You need to find the number of pairs (i, j) , i != j, such that A[i]+A[j] ≤ X.

To understand the problem better, look at this example

Given array - 1 2 2 3 4
X = 4

You have to find pairs of a[i] and a[j] where i!=j and a[i] + a[j] <= X
Let's rearrange the equation to make it easier to solve.

We get, a[j] <= X - a[i]
What this means is, if we run an upper_bound on a sorted array, we'll get the index of the jth element where this condition holds. But it's possible that the ith value is inside this range, so we check if a[i] <= X - a[i]. If yes, subtract one from the value we got. Then increment the count with the updated value.

Let's solve the example above
`1 2 2 3 4 | X = 4`
Pairs possible are
```
[1, 2]
[1, 2]
[1, 3]
[2, 1]
[2, 2]
[2, 1]
[2, 2]
[3, 1]
```

### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

    int n, x; cin >> n >> x;

    vector<int> v(n);

    for(int i = 0; i < n; i++) cin >> v[i];

  

    // Sort the vector

    sort(v.begin(), v.end());

  

    int count = 0;

    // v[j] <= x - v[i]

    for(int i = 0; i < n; i++) {

        int j = upper_bound(v.begin(), v.end(), x - v[i]) - v.begin(); // Find the index (count) of the elements that satisfy the above equation

        if(v[i] <= x - v[i]) j--; // X lies inside the range, so it's pairing up with itself. Subtract 1 to fix this

        count += j;

    }

  

    cout << count << endl;

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