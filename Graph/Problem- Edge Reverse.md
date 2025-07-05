Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Graph]], [[01BFS]]

In this [[DSA]] problem, given a directed graph with NN vertices and MM edges.

What is the minimum number of edges needed to reverse in order to have at least one path from vertex 1 to vertex N, where the vertices are numbered from 1 to N ?

##### Input Format

The first line contains TT - the number of test cases.

The first line of each test case contains two space-separated integers N and M, denoting the number of vertices and the number of edges in the graph respectively. The ith line of the next M lines of each test case contains two space-separated integers Xi​ and Yi​, denoting that the ith edge connects vertices from Xi​ to Yi​.

##### Output Format

For each test case, In a single line, print the minimum number of edges we need to revert. If there is no way of having at least one path from 1 to N, print -1.

##### Constraints

1≤T≤101≤T≤10 1≤N,M≤1051≤N,M≤105 1 ≤ XiXi​, YiYi​ ≤ NN There can be multiple edges connecting the same pair of vertices, There can be self-loops too i.e. Xi​ = Yi​

##### Sample Input 1

1 7 7 1 2 3 2 3 4 7 4 6 2 5 6 7 5

##### Sample Output 1

2

##### Note

We can consider two paths from 1 to 7:

1−2−3−4−71−2−3−4−7 1−2−6−5−71−2−6−5−7

In the first one we need to revert edges (3−2),(7−4)(3−2),(7−4). In the second one - (6−2),(5−6),(7−5)(6−2),(5−6),(7−5). So the answer is min(2,3)=2min(2,3)=2.

## Approach 
This was a nice problem. When adding edges to our adjacency list, we still want to add an edge to both a and b. But, if a->b then to a we'll add {b,0}, as in no penalty distance since there is a direct edge there. But to b, we'll add {a,1} where 1 is essentially the cost of manufacturing an edge there. Then, it's just normal 01BFS.

### Code 
``` cpp
#include<bits/stdc++.h>
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

typedef pair<int, int> state;

vector<vector<state>> g;
vector<bool> visited;
vector<int> dist;

int n, m;

void bfs(int st) {
    deque<int> dq;
    dq.push_back(st);
    dist[st] = 0;

    while(!dq.empty()) {
        int curr = dq.front();
        dq.pop_front();
        
        if(visited[curr]) continue;
        visited[curr] = 1;

        for(state n: g[curr]) {
            int neighbour = n.first;
            int cost = n.second;

            if(dist[curr] + cost < dist[neighbour]) {
                dist[neighbour] = dist[curr] + cost;
                if(cost == 0) dq.push_front(neighbour);
                else dq.push_back(neighbour);
            }
        }
    }

}

void solve() {
    cin >> n >> m;
    g.assign(n+1,{});
    dist.assign(n+1, INT_MAX);
    visited.assign(n+1, 0);

    for(int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        g[a].push_back({b, 0});
        g[b].push_back({a, 1});
    }

    int st = 1, end = n;
    bfs(st);

    cout << (dist[end] == INT_MAX ? -1 : dist[end]) << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    cin >> _t;

    while(_t--) solve();
    return 0;
}

```