Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Prim's Algorithm]], [[Graph]], [[MST]]

In this [[DSA]] problem, you are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the **manhattan distance** between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = `[[0,0],[2,2],[3,10],[5,2],[7,0]]`
**Output:** 20
**Explanation:** 
![](https://assets.leetcode.com/uploads/2020/08/26/c.png)
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.

**Example 2:**

**Input:** points = `[[3,12],[-2,5],[-4,1]]`
**Output:** 18

**Constraints:**

- `1 <= points.length <= 1000`
- `-106 <= xi, yi <= 106`
- All pairs `(xi, yi)` are distinct.

## Approach 
We will use [[Prim's Algorithm]] to solve this problem. To achieve that, we will need a [[priority queue]] and an [[Unordered Set]] to keep track of the visited points. And we will stop after the number of visited points in the becomes = n. The priority queue will store elements in a pair of distance, destination. 

Since we don't care about drawing the edges, we don't need to know where the edge came from as long as it's not repeated (which is taken care of with the visited set). So we will just pop the minimum unvisited point from the heap and add it's distance to the result. Then we push all the neighbours of the current point along with the distance to the heap. And then the cycle repeats. 

### Code 
```cpp
class Solution {
public:
    vector<vector<pair<int, int>>> adj;
    
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        adj.resize(n);

        for(int i = 0; i < n; i++) {
            int x1 = points[i][0];
            int y1 = points[i][1];

            for(int j = i+1; j < n; j++) {
                int x2 = points[j][0];
                int y2 = points[j][1];

                int dist = abs(x1-x2) + abs(y1-y2);
                adj[i].push_back({dist, j});
                adj[j].push_back({dist, i});
            }
        }

        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
        unordered_set<int> visited;
        
        pq.push({0, 0});
        int res = 0;

        while(visited.size() < n) {
            auto [cost, node] = pq.top();
            pq.pop();

            if(visited.count(node)) continue;
            visited.insert(node);
            res += cost;

            for(auto &[dist, nb]: adj[node]) {
                pq.push({dist, nb});
            }
        }

        return res;
    }
};
```