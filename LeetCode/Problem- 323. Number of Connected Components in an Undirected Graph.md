Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Component Numbering]], [[BFS]]

In this [[DSA]] problem, there is an undirected graph with `n` nodes. There is also an `edges` array, where `edges[i] = [a, b]` means that there is an edge between node `a`and node `b` in the graph.

The nodes are numbered from `0` to `n - 1`.

Return the total number of connected components in that graph.

**Example 1:**

```java
Input:
n=3
edges=[[0,1], [0,2]]

Output:
1
```

**Example 2:**

```java
Input:
n=6
edges=[[0,1], [1,2], [2,3], [4,5]]

Output:
2
```

**Constraints:**

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1) / 2`

## Approach 
Very simple problem. It's just component numbering.

### Code 
``` cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> labels;

    void bfs(int node, int c) {
        queue<int> q;
        q.push(node);

        while (!q.empty()) {
            int curr = q.front();
            q.pop();
            labels[curr] = c;

            for (int nb : g[curr]) {
                if (labels[nb] == 0) {
                    q.push(nb);
                }
            }
        }
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        g.resize(n);
        labels.assign(n, 0);

        for (auto& edge : edges) {
            g[edge[0]].push_back(edge[1]);
            g[edge[1]].push_back(edge[0]);
        }

        int comp = 0;
        for (int i = 0; i < n; i++) {
            if (labels[i] == 0) {
                comp++;
                bfs(i, comp);
            }
        }

        return comp;
    }
};
```
