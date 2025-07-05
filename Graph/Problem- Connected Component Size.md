Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[BFS]], [[Graph]], [[Component Numbering]]

In this [[DSA]] problem, you have a 2-D array of size **N x M**. Consider connected **0**s (which share a common edge) as one single component and **1**s as walls. Replace **0**s with the size of the connected component but if the size of the component is one, then leave it with **0**.

##### Input Format

The first line contains a single integer **t**, the number of test cases.  
For each test case, the first line contains two integers **N** and **M** and then there are N lines containing M 0s and 1s, representing a N x M binary matrix.

##### Output Format

For each test case, print the final matrix after replacing all the 0s accordingly.

##### Constraints

1 ≤ Sum of (N x M) over all test cases ≤ 2 x 105  
0 ≤ Ai ≤ 1

##### Sample Input 1

2 2 2 0 1 1 0 6 5 1 0 0 1 0 0 1 0 0 0 0 0 1 1 0 0 1 1 0 1 1 1 1 1 1 0 1 0 0 0

##### Sample Output 1

0 1 1 0 1 7 7 1 7 4 1 7 7 7 4 4 1 1 7 4 1 1 0 1 1 1 1 1 1 0 1 3 3 3

##### Note

In the first test case, we have only 2 components and both have size 1. So nothing is replaced. In the second test case, we have a total of 5 components of size 7, 4, 3, 1, 1 respectively. So all the 0s are replaced accordingly.

## Approach 
We first find the components in the 2D matrix. I have done it using BFS here because I wanted to see how it would work. Turns out the component numbering part is exactly the same. Just the logic of selection of nodes to traverse is different because of BFS.

Then we find the frequency of each component.
Then if the frequency of a component that a cell belongs to isn't 1, we rewrite it with the frq of the component it belongs to. Else we rewrite it as 0.

### Code 
``` cpp
// #include <bits/stdc++.h>
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

typedef pair<int, int> state;

vector<int> dx = {-1, 0, 1, 0};
vector<int> dy = {0, 1, 0, -1};

int n, m;

vector<state> getNeighbours(vector<vector<int>> &g, vector<vector<bool>> &visited, state s) {
    vector<state> neighbours;
    for (int i = 0; i < 4; i++) {
        int newRow = s.first + dx[i];
        int newCol = s.second + dy[i];

        if (
            newRow < 0 || newRow >= n ||
            newCol < 0 || newCol >= m ||
            g[newRow][newCol] == 1 ||
            visited[newRow][newCol] == 1
        ) continue;

        neighbours.push_back({newRow, newCol});
    }
    return neighbours;
}

void bfs(vector<vector<int>> &g, vector<vector<bool>> &visited, int i, int j, vector<vector<int>> &comps, int c) {
    queue<state> q;
    q.push({i, j});

    while (!q.empty()) {
        state next = q.front();
        q.pop();
        visited[next.first][next.second] = 1;
        comps[next.first][next.second] = c;

        for (state s : getNeighbours(g, visited, next)) {
            q.push(s);
        }
    }
}

void solve() {
    cin >> n >> m;
    vector<vector<int>> g(n, vector<int>(m));
    vector<vector<int>> comps(n, vector<int>(m));
    vector<vector<bool>> visited(n, vector<bool>(m, false));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> g[i][j];
        }
    }

    int c = 1;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && g[i][j] != 1) {
                c++;
                bfs(g, visited, i, j, comps, c);
            }
        }
    }

    unordered_map<int, int> frq;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (g[i][j] != 1) {
                frq[comps[i][j]]++;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (g[i][j] != 1) {

```