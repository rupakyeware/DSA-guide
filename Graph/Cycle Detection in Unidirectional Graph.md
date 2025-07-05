In [[Graph]], there's a handy technique to detect and print cycles using labels or 'colours'.
Instead of using a visited vector, we'll replace it with this labels vector.

In the beginning, the label of all the nodes will be 1.
When we start traversing its neighbours, its label will be 2. And when completely finish exploring all its neighbours, its label changes to 3.

When exploring a neighbour, if we encounter a node with a label of 2, it means we have reached a node which hasn't finished exploring its neighbours fully. So there's a cycle.

Then I found a simpler way to print the cycle than Vivek sir's method of keeping a track of the parent and then reversing the list.

The node with which we detect the cycle essentially becomes the start of the cycle. So we change its label to 3 and then recursively go on printing the neighbour of each node whose label is 2. This way, we don't have to maintain the parents or have to perform any reversal either.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

int n, m;
vector<vector<int>> g;
vector<int> labels;
bool isCycle = 0;

void printCycle(int node) {
    cout << node << " ";
    for(int n: g[node]) {
        if(labels[n] == 2) printCycle(n);
    }
}

void dfs(int node) {
    labels[node] = 2;
    if(isCycle) return;

    for(int n: g[node]) {
        if(labels[n] == 1) {
            dfs(n);
        }
        else if(labels[n] == 3) continue;
        else {
            isCycle = 1;
            labels[n] = 3;
            printCycle(n);
            return;
        }
    }

    labels[node] = 3;
}

void solve() {
    cin >> n >> m;
    g.resize(n+1);
    labels.assign(n+1, 1);

    for(int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        g[a].push_back(b);
    }

    int st; cin >> st;
    dfs(st);

    cout << (isCycle ? "\nYES" : "NO") << endl;
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
