Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Intervals]], [[Array]]

In this [[DSA]] problem, you are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti`and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` _after the insertion_.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

**Example 1:**

**Input:** intervals = `[[1,3],[6,9]],` newInterval = [2,5]
**Output:** `[[1,5],[6,9]]`

**Example 2:**

**Input:** intervals = `[[1,2],[3,5],[6,7],[8,10],[12,16]]`, newInterval = [4,8]
**Output:** `[[1,2],[3,10],[12,16]]`
**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`

## Approach 
We want to compare each interval with the new interval and perform some actions based on the conditions.
The conditions can be as follows-
1. If current interval lies before new interval- just push current int to ans 
2. Else if current interval lies after new interval- 
	1. If new interval hasn't already been placed- push it to ans 
	2. push current int to ans 
3. Else- new interval intersects with current int so adjust its bounds 

### Code 
``` cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> ans;
        bool isPlaced = false; // keeps track of if the new interval has already been placed

        for(auto &intv: intervals) { // for each interval in the intervals vector
            if(newInterval[0] > intv[1]) ans.push_back(intv); // if new interval lies after current interval, just push curr interval to ans
            else if(newInterval[1] < intv[0]) { // if new interval lies before curr interval
                if(!isPlaced) { // if new interval hasn't been placed yet, place it
                    ans.push_back(newInterval);
                    isPlaced = true;
                }
                ans.push_back(intv); // place the curr interval
            }
            else { // new interval has to be merged with curr interval
                newInterval[0] = min(newInterval[0], intv[0]); // take min of 1st end of both intervals
                newInterval[1] = max(newInterval[1], intv[1]); // take max of 2nd end of both intervals
            }
        }

        if(!isPlaced) ans.push_back(newInterval); // if new interval still hasn't been placed, place it

        return ans;
    }
};
```