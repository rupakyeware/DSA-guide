In [[Graph]], we can perform a [[DFS]] to find all the traversable nodes.
But doing that in a brute force way would take O(N * (N + M)) time.

But using [[Component Numbering]], that can be reduced to O(N+M)

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

vector<vector<int>> graph; // to convert input into adj list
vector<int> components;

// Traverse all neighbours of node and label them as comp
void dfs(int node, int comp) {
    components[node] = comp; // label node

    for(auto i: graph[node]) { // traverse its neighbours
        if(!components[i]) dfs(i, comp);
    }
}

void solve() {
    int n, m;
    cin >> n >> m;
    graph.resize(n+1);
    components.assign(n+1, 0);

    for(int i = 0; i < m; i++) { // O(M)
        int x, y;
        cin >> x >> y;
        // need to push into both xth and yth vectors as it's a two way reln
        graph[x].push_back(y);
        graph[y].push_back(x);
    }

    // perform component labelling
    int c = 1;
    for(int i = 1; i <= n; i++) { // O(N + M)
        if(!components[i]) { // if we haven't visited current node yet
            dfs(i, c); // perform dfs on
            c++; // increment label value of next component
        }
    }
    
    vector<int> frq(n+1, 0); // frq matrix to store frq of each components
    
    // O(N)
    for(int i = 1; i <= n; i++) frq[components[i]]++; // increment frq of ith component

    // O(N)
    for(int i = 1; i <= n; i++) {
        cout << frq[components[i]] << endl;
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