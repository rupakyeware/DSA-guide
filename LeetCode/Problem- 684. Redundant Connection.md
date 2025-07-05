Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Disjoint Set Union]], [[Graph]], [[Path Compression]]

In this [[DSA]] problem, in this problem, a tree is an **undirected graph** that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1`to `n`, with one additional edge added. The added edge has two **different**vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return _an edge that can be removed so that the resulting graph is a tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the input.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)

**Input:** edges = `[[1,2],[1,3],[2,3]]`
**Output:** [2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

**Input:** edges = `[[1,2],[2,3],[3,4],[1,4],[1,5]]`
**Output:** [1,4]

**Constraints:**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ai < bi <= edges.length`
- `ai != bi`
- There are no repeated edges.
- The given graph is connected.

## Approach 
I tried to solve this problem by myself using a hashmap and then a 2D vector of booleans but I wasn't able to.
Then when I learned about Disjoint Set Union and path compression, it blew my mind. It's so cool the way over time the findParent function becomes O(1) because we compress the path to its root parent!

While adding an edge between two nodes, we want to get their root parents. If their parents aren't the same, we get the rank of their parents and then add the lower ranked parent to the set of the higher ranked parent. This ensures that the graph is kept as shallow as possible.

But if the parents aren't the same, it means we have found the extra edge which will cause a cycle. 

### Code 
```cpp
class Solution {
public:
    vector<int> par;
    vector<int> rank;

    int findParent(int n) {
        int p = par[n]; // get parent of node
        while(p != par[p]) { // if parent of node is not the same as the node itself
            par[p] = par[par[p]]; // move parent of p one node up
            p = par[p]; // move p to its parent
        }

        return p; // return the root parent
    }

    bool tryUnion(int n1, int n2) {
        int p1 = findParent(n1); // find root parent of n1
        int p2 = findParent(n2); // find root parents of n2

        if(p1 == p2) return false; // if both parents are the same, we have our answer

        if(rank[p1] >= rank[p2]) { // p1 has more child nodes than p2, so attach par of p2 to p1
            par[p2] = p1;
            rank[p1] += rank[p2];
        }
        else { // p2 has more child nodes than p1 so do the reverse for p2
            par[p1] = p2;
            rank[p2] += rank[p1];
        }

        return true; // union set was formed
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        par = vector<int> (n+1);
        rank = vector<int> (n+1, 1);

        for(int i = 1; i <= n; i++) par[i] = i;

        for(auto &edge: edges) {
            // for each edge, try to make a union set
            if(!tryUnion(edge[0], edge[1])) { // if union isn't possible, we have our answer
                return {edge[0],edge[1]};
            }
        }

        return {};
    }
};
```