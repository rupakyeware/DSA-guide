Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Hierholzer]], [[Eulerian Path]], [[DFS]]

In this [[DSA]] problem, you are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

- For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

**Input:** tickets = `[["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]`
**Output:** ["JFK","MUC","LHR","SFO","SJC"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

**Input:** tickets = `[["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]`
**Output:** ["JFK","ATL","JFK","SFO","ATL","SFO"]
**Explanation:** Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.

**Constraints:**

- `1 <= tickets.length <= 300`
- `tickets[i].length == 2`
- `fromi.length == 3`
- `toi.length == 3`
- `fromi` and `toi` consist of uppercase English letters.
- `fromi != toi`
## Approach 
We want to find a eulerian path such that the answer vector is in lexicographic order. The way we achieve that is by using [[Hierholzer]]'s algorithm.

We will do the following-
1. Make the adjacency list. 
2. Sort each vector of adjacency list. 
3. Start [[DFS]] from the source node 
4. While the neighbours vector is not empty
	1. Pop next neighbour from list (which will also be the next lexicographically smallest destination)
	2. Perform [[DFS]] on it.
	3. Continue with remaining neighbours the same way 
5. Add current source to answer vector 

Now because of the way we've written the dfs, the adding of sources to the answer vector will be done in reverse order. So the last node will be added first.
Hence, we want to reverse it before passing it as the answer. 

The algorithm itself isn't that difficult. Just knowing how to manipulate according to different problems might be a challenge.

### Code 
``` cpp
class Solution {
public:
    unordered_map<string, deque<string>> adj;
    vector<string> res;

    void dfs(string curr, unordered_map<string, deque<string>> &adj, vector<string> &res) {
        while(!adj[curr].empty()) {
            string dest = adj[curr].front();
            adj[curr].pop_front();
            dfs(dest, adj, res);
        }

        res.push_back(curr);
    }

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for(auto &tkt: tickets) {
            adj[tkt[0]].push_back(tkt[1]);
        }

        for(auto &[src, dests]: adj) {
            sort(dests.begin(), dests.end());
        }

        string st = "JFK";
        dfs(st, adj, res);

        reverse(res.begin(), res.end());
        return res;
    }
};
```