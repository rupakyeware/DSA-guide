Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[DFS]], [[Cycle Detection in Unidirectional Graph]]

In this [[DSA]] problem, there are a total of `numCourses` courses you have to take, labeled from `0`to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = `[[1,0]]`
**Output:** true
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = `[[1,0],[0,1]]`
**Output:** false
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

## Approach
It was pretty easy to come up with the approach once I drew out the graph in the examples. It's a cycle detection algorithm so each node can have one of 3 values- 1, 2 or 3.
When it's unvisited, its value will be 1. When we start visiting its neighbours, its value will be 2. And when we're done visiting all its neighbours, its value will be 3. While traversing a node (whose value will be 2 at that time), if we encounter another node whose value is 2, it means a cycle has been found and it's not possible to finish all the courses so we return false.

Else if all the courses are traversed, we return true;

### Code 
```cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> labels;
    bool isPossible = true;

    void dfs(int node) {
        if(!isPossible) return;

        labels[node] = 2;

        for(int nb: g[node]) {
            if(labels[nb] == 1) dfs(nb);
            else if(labels[nb] == 2) {
                isPossible = false;
                return;
            }
        }

        labels[node] = 3;
    }

    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        g.resize(numCourses);
        labels.assign(numCourses, 1);

        int p = prerequisites.size();
        if(p <= 0) return true;

        for(auto &edge: prerequisites) {
            g[edge[0]].push_back(edge[1]);
        }

        for(int i = 0; i < numCourses; i++) {
            if(isPossible && labels[i] == 1) dfs(i);
        }

        return isPossible;
    }
};
```