Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Intervals]], [[priority queue]], [[Array]]

In this [[DSA]] problem, you are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `ith` interval starting at `lefti` and ending at `righti` **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `jth` query is the **size of the smallest interval** `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return _an array containing the answers to the queries_.

**Example 1:**

**Input:** intervals = `[[1,4],[2,4],[3,6],[4,4]]`, queries = [2,3,4,5]
**Output:** [3,3,1,4]
**Explanation:** The queries are processed as follows:
- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.

**Example 2:**

**Input:** intervals = `[[2,3],[2,5],[1,8],[20,25]]`, queries = [2,19,5,22]
**Output:** [2,-1,4,6]
**Explanation:** The queries are processed as follows:
- Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.

**Constraints:**

- `1 <= intervals.length <= 105`
- `1 <= queries.length <= 105`
- `intervals[i].length == 2`
- `1 <= lefti <= righti <= 107`
- `1 <= queries[j] <= 107`

## Approach 
As all [[Intervals]] problems, first we'll sort the intervals vector, But here, we also need to sort the queries vector to solve this in less than exponential time. Then, we'll maintain a min heap of pairs where each pair will store the minimum interval and end of the minimum interval. 

For each query, first we'll push all the valid intervals to the priority queue. An interval is valid is its start is less than or equal to the value of the query. Then, we need to remove all the pairs in the heap whose end value (aka the second value in the pair) is less than the current query. This is because those intervals end before the current query's required time. After that, in the priority queue, only those intervals will remain which are valid for the current query. And hence, we just append the top of heap to the ans vector.

But we can't just directly append to the ans vector as the answer is required in the order of the original queries vector. So we'll store the original queries and their indexes and then refer to the that index while inserting the answer to the ans vector. This'll be clear from the code. 

### Code 
```cpp
class Solution {
public:
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
        int qS = queries.size();
        vector<pair<int,int>> queriesIdx;
        for(int i = 0; i < qS; i++) {
            queriesIdx.push_back({queries[i], i});
        }

        sort(queriesIdx.begin(), queriesIdx.end());
        sort(intervals.begin(), intervals.end());

        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
        int curr = 0, n = intervals.size();
        vector<int> ans(qS, -1);

        for(auto &q: queriesIdx) {
            while(curr < n && intervals[curr][0] <= q.first) {
                pq.push({intervals[curr][1]-intervals[curr][0]+1, intervals[curr][1]});
                curr++;
            }

            while(!pq.empty() && pq.top().second < q.first) {
                pq.pop();
            }

            
            if(pq.empty()) ans[q.second] = -1;
            else ans[q.second] = pq.top().first;
        }

        return ans;
    }
};
```