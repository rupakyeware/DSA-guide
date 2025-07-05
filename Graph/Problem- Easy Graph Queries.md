Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[DFS]], [[Component Numbering]]

In this [[DSA]] problem, you are given an undirected graph _G_ with _N_ nodes and _M_ edges. You've to answer _Q_ queries. Each query is either of the following two types:

- 1 _X_: Print the size of the connected components containing node _X_.
- 2 _X_ _Y_: Print ‘YES’ (without quotes) if node _X_ and _Y_ belong to the same connected component, else print ‘NO’ (without quotes).

##### Input Format

The first line of input contains three space-separated integers _N_, _M_, and _Q_ (1 ≤ _N, M, Q_ ≤ 105).  
Next _M_ lines contain two space-separated integers _u_ and _v_ (1 ≤ _u, v_ ≤ _N_).  
Each of the next _Q_ lines contains queries of one of the types as described in the statement.

##### Output Format

Print _Q_ lines as the answer to the _Q_ queries, each on a new line.

##### Constraints

##### Sample Input 1

6 5 5 1 2 2 3 1 3 4 4 5 6 1 2 1 4 2 3 4 1 5 2 5 6

##### Sample Output 1

3 1 NO 2 YES

## Approach 
Just find which component each node belongs to, then make a frequency vector of all the components. Then answer the queries.

### Code 
``` cpp
#include <bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

vector<vector<int>> g;
vector<int> comp;
vector<bool> visited;

void dfs(int node, int component) {
    comp[node] = component;
    visited[node] = 1;

    for(int i: g[node]) {
        if(!visited[i]) dfs(i, component);
    }
}

void solve() {
    int n, m, q; cin >> n >> m >> q;
    g.resize(n+1);
    visited.assign(n+1, 0);
    comp.resize(n+1);
    
    for(int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        g[a].push_back(b);
        g[b].push_back(a);
    }

    int component = 1;
    for(int i = 1; i <= n; i++) {
        if(!visited[i]) {
            dfs(i, component);
            component++;
        }
    }

    vector<int> frq(n+1);
    for(int i = 1; i <= n; i++) {
        frq[comp[i]]++;
    }

    while(q--) {
        int type; cin >> type;
        if(type == 1) {
            int x; cin >> x;
            cout << frq[comp[x]] << endl;
        }
        else {
            int x, y; cin >> x >> y;
            cout << (comp[x] == comp[y] ? "YES" : "NO") << endl;
        }
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