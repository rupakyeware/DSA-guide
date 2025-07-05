Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Cycle Detection in Unidirectional Graph]], [[BFS]]

In this [[DSA]] problem, there are a total of `numCourses` courses you have to take, labeled from `0`to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return _the ordering of courses you should take to finish all courses_. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example 1:**

**Input:** numCourses = 2, prerequisites = `[[1,0]]`
**Output:** [0,1]
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Example 2:**

**Input:** numCourses = 4, prerequisites = `[[1,0],[2,0],[3,1],[3,2]]`
**Output:** [0,2,1,3]
**Explanation:** There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

**Example 3:**

**Input:** numCourses = 1, prerequisites = `[]`
**Output:** [0]

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- All the pairs `[ai, bi]` are **distinct**.

## Approach 
Literally only 2 lines have to be added to the code of [[Problem- 207. Course Schedule]]. We need a vector that'll store the order. And for that we need a way to determine when the neighbours (prerequisites) of a node (course) have been fully explored, which we already. So after we label a node as 3, we push the number of that course to the order vector. And before returning the vector, we just check if we found a cycle in the graph. If we did, we return an empty vector, else we return the order vector.

### Code 
``` cpp
class Solution {
public:
    vector<int> labels;
    bool canFinish = true;
    vector<vector<int>> g;
    vector<int> order;

    void dfs(int course) {
        if(!canFinish) return;

        labels[course] = 2;

        for(int nb: g[course]) {
            if(labels[nb] == 2) {
                canFinish = false;
                return;
            } 
            else if(labels[nb] == 1) dfs(nb);
        }

        labels[course] = 3;
        order.push_back(course);
    }

    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        int pS = prerequisites.size();
        g.resize(numCourses);
        labels.assign(numCourses, 1);

        for(auto &state: prerequisites) {
            g[state[0]].push_back(state[1]);
        }

        for(int i = 0; i < numCourses; i++) {
            if(canFinish && labels[i] == 1) dfs(i);
        }

        if(!canFinish) return {};
        return order;
    }
};
```