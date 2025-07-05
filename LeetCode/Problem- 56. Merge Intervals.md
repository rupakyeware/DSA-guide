Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Intervals]], [[Array]]

In this [[DSA]] problem, given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = `[[1,3],[2,6],[8,10],[15,18]]`
**Output:** `[[1,6],[8,10],[15,18]]`
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**

**Input:** intervals = `[[1,4],[4,5]]`
**Output:** `[[1,5]]`
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

## Approach-
This is similar to [[Problem- 57. Insert Interval]] except instead of getting a interval as an input, we kinda have to create it.

We will maintain a pair of ints which will be where we store the temporary interval. In the beginning, we'll set the first interval as the temporary interval. 

If the temp interval lies before the current interval, we'll push the temporary interval to the ans, and set the new temporary interval as the current interval.

Else, it means that the current interval overlaps with the temp interval, so we adjust its bounds.

### Code 
```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());

        pair<int,int> currIntv = {-1, -1};
        vector<vector<int>> ans;

        for(auto &intv: intervals) {
            if(currIntv.first == -1) {
                currIntv.first = intv[0];
                currIntv.second = intv[1];
            }
            else if(currIntv.second < intv[0]) {
                if(currIntv.first != -1) {
                    ans.push_back({currIntv.first, currIntv.second});
                    currIntv.first = intv[0];
                    currIntv.second = intv[1];
                }
            }
            else {
                currIntv.first = min(currIntv.first, intv[0]);
                currIntv.second = max(currIntv.second, intv[1]);
            }
        }

        if(currIntv.first != -1) ans.push_back({currIntv.first, currIntv.second});

        return ans;
    }
};
```