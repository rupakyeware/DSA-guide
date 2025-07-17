Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Bellman-Ford]], [[Graph]]

In this [[DSA]] problem, there are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst`_with at most_ `k` _stops._ If there is no such route, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-3drawio.png)

**Input:** n = 4, flights = `[[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]`, src = 0, dst = 3, k = 1
**Output:** 700
**Explanation:**
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png)

**Input:** n = 3, flights = `[[0,1,100],[1,2,100],[0,2,500]]`, src = 0, dst = 2, k = 1
**Output:** 200
**Explanation:**
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-2drawio.png)

**Input:** n = 3, flights = [[`0,1,100],[1,2,100],[0,2,500]]`, src = 0, dst = 2, k = 0
**Output:** 500
**Explanation:**
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.

**Constraints:**

- `1 <= n <= 100`
- `0 <= flights.length <= (n * (n - 1) / 2)`
- `flights[i].length == 3`
- `0 <= fromi, toi < n`
- `fromi != toi`
- `1 <= pricei <= 104`
- There will not be any multiple flights between two cities.
- `0 <= src, dst, k < n`
- `src != dst`

## Approach 
We will be using an implementation of the [[Bellman-Ford]] algorithm where we will loop over the entire graph k+1 times starting with 0 jumps to k jumps. In each iteration, we will find the optimal prices of reachable nodes and update them in a temp vector at runtime. After we are done with the iteration, we'll update that into the dist vector.

### Code 
```cpp
typedef pair<int,int> state;
const int INF = 10010010;


class Solution {
public:    
    vector<int> dist; // to store the previously found prices of nodes
    vector<int> temp; // to store the currently found optimal prices of nodes
    vector<vector<state>> adj; 
    int N;

    void bf(int src, int k) {
        dist[src] = 0;
        temp[src] = 0;

        for(int i = 0; i <= k; i++) { // we will loop over all the edges in the graph k+1 times (meaning 0 to k stops)
            for(int j = 0; j < N; j++) {
                if(dist[j] == INF) continue; // if current node is unreachable right now, don't explore it
                for(auto &[price, dest]: adj[j]) { // for every neighbour of current node

                    // it's important to know when to use the value from which array here.
                    // while checking current distance of a node, we use temp as we don't want to overwrite previously found optimal distances
                    // in the current iteration. 
                    // But to calculate the current distance, we use the dist array as that is the price we have found of the jth element with 
                    // i stops.
                    if(temp[dest] > dist[j] + price) { // if we have found a better path than before
                        temp[dest] = dist[j] + price;
                    }
                }
            }
            dist = temp; // update dist with the newly found optimal prices
        }
    }

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        N = n;
        adj.resize(n);
        dist.assign(n, INF);
        temp.assign(n, INF);
        
        for(auto &flight: flights) {
            adj[flight[0]].push_back({flight[2], flight[1]});
        }

        bf(src, k);

        return (dist[dst] != INF ? dist[dst] : -1);
    }
};
```