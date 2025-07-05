Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Component Numbering]], [[DFS]]

In this [[DSA]] problem, you are given an undirected graph with n nodes, and m edges between them. The goal is to add exactly one edge between two nodes such that the total number of connected components in the graph decreases. Your task is to find out the number of ways to add such edge.

##### Input Format

The first input line has two integers n and m: the number of nodes and edges. The nodes are numbered 1, 2, …, n. After that, there are m lines describing the edges. Each line has two integers a and b: there is an edge between those nodes. An edge always connects two different nodes, and there is at most one edge between any two nodes.

##### Output Format

Print the number of ways to add such edge, described in the statement.

##### Constraints

1 ≤ _n_ ≤ 105  
1 ≤ _m_ ≤ 2 x 105  
1 ≤ _a, b_ ≤ _n_

##### Sample Input 1

5 4 1 2 2 3 1 3 4 5

##### Sample Output 1

6

##### Sample Input 2

4 3 1 2 2 3 3 4

##### Sample Output 2

0

##### Note

**Explanation 1:**  
There are 6 ways to add edge so that the number of connected components in the graph decreases: (1, 4), (1, 5), (2, 4), (2, 5), (3, 4), (3, 5).

**Explanation 2:**  
The given graph is already connected. Even if add any edge, we can't decrease the number of connected components.

## Approach 
It took me a while to come up with the correct approach for this.
We have to find the number of nodes in each component.

Then the number of ways to add an edge using one component and the rest of the graph is
`(total remaining nodes - frq[current component]) * frq[current component]`

I had to really draw everything out to come up with the intuition for this myself. But I'm happy that I was able to do it myself without any external help.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
// #include <bits/stdc++.h>
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
    int n, m; cin >> n >> m;
    g.resize(n+1);
    visited.assign(n+1, 0);
    comp.resize(n+1);
    
    // make adj list
    for(int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        g[a].push_back(b);
        g[b].push_back(a);
    }

    // create component numbered node list
    int component = 1;
    for(int i = 1; i <= n; i++) {
        if(!visited[i]) {
            dfs(i, component);
            component++;
        }
    }

    // find frq of components
    vector<int> frq(n+1);
    for(int i = 1; i <= n; i++) {
        frq[comp[i]]++;
    }

    int remainingNodes = n; // total remaining nodes = n
    int ways = 0;    
    for(int i = 1; i < frq.size(); i++) {
        remainingNodes -= frq[i]; // reduce frq of current component from remainingNodes
        ways += (remainingNodes * frq[i]); // add to ways -> frq of current component * number of total remaining nodes
    }

    cout << ways << endl;
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

