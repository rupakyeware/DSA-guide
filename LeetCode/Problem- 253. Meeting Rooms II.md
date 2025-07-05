Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Intervals]], [[Array]], [[priority queue]]

In this [[DSA]] problem, given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...] (start_i < end_i)`, find the minimum number of days required to schedule all meetings without any conflicts.

**Note:** (0,8),(8,10) is not considered a conflict at 8.

**Example 1:**

```java
Input: intervals = [(0,40),(5,10),(15,20)]

Output: 2
```

Explanation:  
day1: (0,40)  
day2: (5,10),(15,20)

**Example 2:**

```java
Input: intervals = [(4,9)]

Output: 1
```

**Constraints:**

- `0 <= intervals.length <= 500`
- `0 <= intervals[i].start < intervals[i].end <= 1,000,000`

## Approach 1- Bruteforce
This is the approach I came up with which takes O(n^2) time. We'll maintain a map of the prevEnds of days we've allocated so far. When we get a new interval, we'll iterate through the prevEnds map to find if we can allocate the current meeting in an already allocated day (intv.start >= prevEnds[i])  and increase the value of that day's prevEnds time to intv.end. Else, we create a new day and set its prevEnds value as intv.end.

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
    int minMeetingRooms(vector<Interval>& intervals) {
        sort(intervals.begin(), intervals.end(), [](Interval a, Interval b) {
            return a.start < b.start;
        });

        int n = intervals.size();
        vector<int> prevEnds(n, -1);
        int setAt = 0;

        for(auto &intv: intervals) {
            bool isFit = false;
            for(int i = 0; i < setAt; i++) {
                if(intv.start >= prevEnds[i]) {
                    prevEnds[i] = intv.end;
                    isFit = true;
                    break;
                }
            }
            if(!isFit) {
                prevEnds[setAt] = intv.end;
                setAt++;
            }
        }

        return setAt;
    }
};
```

## Approach 2- Priority Queue
This is the optimal approach which takes O(nlogn) time.
At any point of time, to find out if the current interval has an available day or not, we just need to find out what is the value of the day with the minimum end time. So for that, we'll maintain a min heap. 

If the heap is not empty and intv.start >= pq.top() then it means there's a day available where the current meeting can be allocated so we pop from the queue and push the end time of the current interval to the heap.

Else we just push intv.end into the heap, which means a new day had to be created to allocate the current meeting.

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
    int minMeetingRooms(vector<Interval>& intervals) {
        // Sort intervals by start time
        sort(intervals.begin(), intervals.end(), [](Interval a, Interval b) {
            return a.start < b.start;
        });

        // Min heap to track the end times of ongoing meetings
        priority_queue<int, vector<int>, greater<int>> pq;

        for (auto& intv : intervals) {
            // If the current meeting starts after the earliest ended one, reuse the room
            if (!pq.empty() && intv.start >= pq.top()) {
                pq.pop();
            }
            pq.push(intv.end); // Push current meeting's end time
        }

        // The size of the heap is the number of meeting rooms required
        return pq.size();
    }
};
```
