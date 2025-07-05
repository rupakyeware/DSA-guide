Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Intervals]]

In this [[DSA]] problem, given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...] (start_i < end_i)`, determine if a person could add all meetings to their schedule without any conflicts.

**Example 1:**

```java
Input: intervals = [(0,30),(5,10),(15,20)]

Output: false
```

Explanation:

- `(0,30)` and `(5,10)` will conflict
- `(0,30)` and `(15,20)` will conflict

**Example 2:**

```java
Input: intervals = [(5,8),(9,15)]

Output: true
```

**Note:**

- (0,8),(8,10) is not considered a conflict at 8

**Constraints:**

- `0 <= intervals.length <= 500`
- `0 <= intervals[i].start < intervals[i].end <= 1,000,000`

## Approach
So we will use an approach similar to [[Problem- 435. Non-overlapping intervals]] where we will sort the vector and then just keep a track of the prevEnd. Here, the sorting was done using a custom lambda function (I learnt something new) as a user-defined class was used to store the intervals. 

### Code 
``` cpp
/**
 * Definition of Interval:
 * class Interval {
 * public:
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class Solution {
public:
    bool canAttendMeetings(vector<Interval>& intervals) {
        sort(intervals.begin(), intervals.end(), [](Interval a, Interval b) {
            return a.start < b.start;
        });
        
        int n = intervals.size();
        int prevEnd;
        if(n) prevEnd = intervals[0].end;

        for(int i = 1; i < n; i++) {
            if(intervals[i].start < prevEnd) return false;
            else prevEnd = intervals[i].end;
        }

        return true;
    }
};
```