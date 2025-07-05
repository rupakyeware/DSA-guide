Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Greedy]], [[Intervals]], [[Array]]

In this [[DSA]] problem, given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Note** that intervals which only touch at a point are **non-overlapping**. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

**Example 1:**

**Input:** intervals = `[[1,2],[2,3],[3,4],[1,3]]`
**Output:** 1
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**

**Input:** intervals =` [[1,2],[1,2],[1,2]]`
**Output:** 2
**Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.

**Example 3:**

**Input:** intervals = `[[1,2],[2,3`]]
**Output:** 0
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

**Constraints:**

- `1 <= intervals.length <= 105`
- `intervals[i].length == 2`
- `-5 * 104 <= starti < endi <= 5 * 104`

## Approach
We have to take a greedy approach for this problem. I tried to think of a way to pick the optimal interval to delete and failed. Turns out the solution was this simple lol. 

So since we'll be sorting the array, we can safely assume that the starts of each interval won't intersect. We only have to handle the ends of each interval to check if they are intersecting or not. So we'll store the end of the previous interval in a variable and if the end of the current interval is less than prevEnd, it means that it's intersecting. 

Here's where the greedy choice comes in. Since we want to delete the longer interval, we'll set the prevEnd as the minimum of the end of the current interval and prevEnd which will successfully get rid of the longer interval and then we'll increment the count of intervals deleted.

### Code 
```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        
        int prevEnd = intervals[0][1];
        int n = intervals.size();
        int cnt = 0;

        for(int i = 1; i < n; i++) {
            auto &edge = intervals[i];
            if(edge[0] < prevEnd) {
                cnt++;
                prevEnd = min(edge[1], prevEnd);
            }
            else {
                prevEnd = edge[1];
            }
        }

        return cnt;
    }
};
```