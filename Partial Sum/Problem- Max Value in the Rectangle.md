Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Partial Sums]], [[Prefix Sum]]

In this [[DSA]] problem, you are given a 2d-array of dimension _N*M_ and _Q_ queries. In each query five integers _x1, y1, x2, y2, C_ is given, you have to increase the value of each cell in the submatrix with _(x1,y1)_ be the leftmost corner and _(x2,y2)_ be the rightmost corner by _C_. Initially the value of all the cell of the 2d-array is 0.

After all the query is performed, print the maximum value present in the 2d-array and the number of cells with the maximum value.

## Approach
Once I understood the question, it wasn't that hard of a problem to solve. 
First you to need to perform partial sums on the 2D matrix according to the queries given. Then make the final array out of it using the formula for prefix sum. At the same time, keep a track of what the max variable is and it's count and increment or change it accordingly

```
#include "../stdc++.h"

#define endl '\n'

#define int long long

using namespace std;

  

void solve() {

int n, m, q; cin >> n >> m >> q;

vector<vector<int>> v(n+2, vector<int> (m+2, 0)); // Initialize a 2D array of size n x m with all 0s

// We have done n+2 and m+2 becayse

// 1. We are given inputs in 1D. So we have to add one extra row and col each for that

// 2. Adding the partial sum values at the borders requires another extra row and col each in edge cases.

while(q--) {

// Perform the partial sum operations for each query

int x1, y1, x2, y2, c; cin >> x1 >> y1 >> x2 >> y2 >> c;

v[x1][y1] += c;

v[x1][y2+1] += -(c);

v[x2+1][y1] += -(c);

v[x2+1][y2+1] += c;

}

  

// Find final matrix and add increment cnt of max

int max = INT_MIN, cnt = 0;

for(int i = 1; i <= n; i++) {

for(int j = 1; j <= m; j++) {

v[i][j] += v[i-1][j] + v[i][j-1] - v[i-1][j-1];

if(v[i][j] > max) {

max = v[i][j];

cnt = 1;

}

else if(v[i][j] == max) cnt++;

}

}

  

// Print value and count of last element in map

cout << max << " " << cnt << endl;

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