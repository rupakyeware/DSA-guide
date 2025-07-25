Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Djikstra]]

In this [[DSA]] problem, you are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Input:** times = `[[2,1,1],[2,3,1],[3,4,1]]`, n = 4, k = 2
**Output:** 2

**Example 2:**

**Input:** times = `[[1,2,1]]`, n = 2, k = 1
**Output:** 1

**Example 3:**

**Input:** times = `[[1,2,1]]`, n = 2, k = 2
**Output:** -1

**Constraints:**

- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `0 <= wi <= 100`
- All the pairs `(ui, vi)` are **unique**. (i.e., no multiple edges.)

## Approach 
This is just a classical [[Djikstra]]'s algorithm problem where we have to find the minimum time it takes to traverse all the nodes.

After performing djikstra, I'm just returning the max value of my dist vector since the max value will be of the node that was visited last.

### Code 
```cpp
typedef pair<int,int> state;

class Solution {
public:
    vector<vector<state>> adj;
    vector<int> dist;

    void djikstra(int st) {
        priority_queue<state, vector<state>, greater<state>> pq;
        dist[st] = 0;
        pq.push({dist[st], st});

        while(!pq.empty()) {
            state front = pq.top();
            pq.pop();

            for(auto &nb: adj[front.second]) {
                if(dist[nb.second] > dist[front.second] + nb.first) {
                    dist[nb.second] = dist[front.second] + nb.first;
                    pq.push({dist[nb.second], nb.second});
                }
            }
        }
    }

    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        adj.resize(n+1);
        dist.assign(n+1, 1001001);

        for(auto t: times) {
            adj[t[0]].push_back({t[2], t[1]});
        }

        djikstra(k);
        int ans = *max_element(dist.begin()+1, dist.end());

        return (ans != 1001001 ? ans : -1);
    }
};
```