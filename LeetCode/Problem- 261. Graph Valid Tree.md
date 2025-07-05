Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Tree]], [[DFS]], [[Cycle Detection in Undirected Graph]]

In this [[DSA]] problem, given `n` nodes labeled from `0` to `n - 1` and a list of **undirected** edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example 1:**

```java
Input:
n = 5
edges = [[0, 1], [0, 2], [0, 3], [1, 4]]

Output:
true
```

**Example 2:**

```java
Input:
n = 5
edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]

Output:
false
```

**Note:**

- You can assume that no duplicate edges will appear in edges. Since all edges are `undirected`, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.

**Constraints:**

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1) / 2`

## Approach
This was a nice problem. I had to draw everything out and try to come up with different approaches to figure out what might work and what might not. I was also allowing a disconnected graph to be called a valid tree so I had to understand and fix that.

The technique I used is similar to [[Problem- 207. Course Schedule]] where I try to see if there's any cycle in the graph. Then I check if the graph is disconnected anywhere. Another trick I learned with the help of chatGPT is to check if the number of edges is exactly n-1. That way I don't have to go through the visited array again so it saves me O(n) time.

### Code 
``` cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> labels;
    bool isValid = true;

    void dfs(int node, int parent) {
        if (!isValid) return;
        labels[node] = 2;

        for (int nb : g[node]) {
            if (nb == parent) continue;
            else if (labels[nb] == 2) {
                isValid = false;
                return;
            } else {
                dfs(nb, node);
            }
        }
    }

    bool validTree(int n, vector<vector<int>>& edges) {
        g.resize(n);
        labels.assign(n, 1);
        if (edges.size() != n - 1) {
            isValid = false;
        }

        for (auto& edge : edges) {
            g[edge[0]].push_back(edge[1]);
            g[edge[1]].push_back(edge[0]);
        }

        dfs(0, -1);
        return isValid;
    }
};
```
