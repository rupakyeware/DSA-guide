Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[DFS]], [[Graph]], [[Component Numbering]]

In this [[DSA]] problem, Zenithland has n cities and m roads between them. The goal is to construct new roads so that there is a route between any two cities. A road is bidirectional. Your task is to find out the minimum number of roads required.

##### Input Format

The first input line has two integers n and m: the number of cities and roads. The cities are numbered 1, 2, …, n. After that, there are m lines describing the roads. Each line has two integers a and b: there is a road between those cities. A road always connects two different cities, and there is at most one road between any two cities.

##### Output Format

Print the number of minimum roads required.

##### Constraints

1 ≤ _n_ ≤ 105  
1 ≤ _m_ ≤ 2 x 105  
1 ≤ _a, b_ ≤ _n_

##### Sample Input 1

4 2 1 2 3 4

##### Sample Output 1

1

##### Note

Construct a road between cities 1 and 3.

## Approach-
Just need to find the number of components in the graph. The number of roads to build will be the number of different components - 1.

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

vector<vector<int>> g;
vector<bool> visited;
vector<int> assigned;

void dfs(int node, int c) {
    visited[node] = 1;
    assigned[node] = c;

    for(int i : g[node]) {
        if(!visited[i]) {
            dfs(i, c);
        }
    }
}

void solve() {
    int n, m; cin >> n >> m;
    g.resize(n+1);
    visited.assign(n+1, 0);
    assigned.resize(n+1);

    for(int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        g[a].push_back(b);
        g[b].push_back(a);
    }

    int c = 0;
    for(int i = 1; i <= n; i++) {
        if(!visited[i]) {
            c++;
            dfs(i, c);
        }
    }

    cout << c - 1 << endl;
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
