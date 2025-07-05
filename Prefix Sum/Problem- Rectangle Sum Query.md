Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Prefix Sum]]


 In this [[DSA]] problem, you are given a 2D array of dimensions N×M and Q queries. Each query consists of four integers x1,y1,x2,y2x1​,y1​,x2​,y2​ representing the top-left (x1,y1)(x1​,y1​) and bottom-right (x2,y2)(x2​,y2​) coordinates of a submatrix.  
For each query, compute the sum of all elements in the given submatrix, modulo 10^9 + 7.

## Approach - Using 2D Prefix Sum
This problem took a lot of wrong answers to get it correctly along with some help from claude.ai.
The theory behind it is simple. You use the normal 2D prefix sum logic to create the prefix array and then the normal logic to find the values lying the ranges of the query. The trouble was in making sure the values are properly and accurately modded. That took a lot of tries. But I learned the normalize function because of it

In the normalize function, we mod the number, then add mod to it and then mod it again.
Example cases:
- If `x = 15` and `MOD = 7`:
    - `x % MOD = 1`
    - `(1 + 7) % 7 = 1`
- If `x = -5` and `MOD = 7`:
    - `x % MOD = -5`
    - `(-5 + 7) % 7 = 2`
- If `x = 7` and `MOD = 7`:
    - `x % MOD = 0`
    - `(0 + 7) % 7 = 0`

```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

const int MOD = 1e9 + 7;

  

int normalize(int x) {

return ((x % MOD) + MOD) % MOD;

}

  

void solve() {

int n, m, q; cin >> n >> m >> q;

vector<vector<int>> v(n, vector<int> (m, 0));

for(int i = 0; i < n; i++) {

for(int j = 0; j < m; j++) {

cin >> v[i][j];

v[i][j] = normalize(v[i][j]);

}

}

  

// Create the 2D prefix matrix

vector<vector<int>> prefix_v(n + 1, vector<int> (m + 1, 0));

for(int i = 1; i <= n; i++) {

for(int j = 1; j <= m; j++) {

// Prefix of current cell = prefix of upper cell + prefix of left cell - prefix of upper left cell + value of current cell

// prefix_v[i][j] = prefix_v[i-1][j] + prefix_v[i][j-1] - prefix_v[i-1][j-1] + v[i-1][j-1];

int v1 = normalize(prefix_v[i-1][j]);

int v2 = normalize(prefix_v[i][j-1]);

int v3 = normalize(prefix_v[i-1][j-1]);

int v4 = v[i-1][j-1];

  

prefix_v[i][j] = normalize(v1 + v2 - v3 + v4);

}

}

  

while(q--) {

int x1, y1, x2, y2; cin >> x1 >> y1 >> x2 >> y2;

// First calculate area of bottom right point

// Subtract row above top left

// Subtract column before top left

// Add point diagonally to the top left of top left point

// int ans = prefix_v[x2][y2] - prefix_v[x1-1][y2] - prefix_v[x2][y1-1] + prefix_v[x1-1][y1-1];

int v1 = normalize(prefix_v[x2][y2]);

int v2 = normalize(prefix_v[x1-1][y2]);

int v3 = normalize(prefix_v[x2][y1-1]);

int v4 = normalize(prefix_v[x1-1][y1-1]);

int ans = normalize(v1 - v2 - v3 + v4);

cout << ans << endl;

}

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