Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Deque]], [[Sliding Window]]

In this [[DSA]] problem, you are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3
**Output:** [3,3,5,5,6,7]
**Explanation:** 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       **3**
 1 [3  -1  -3] 5  3  6  7       **3**
 1  3 [-1  -3  5] 3  6  7      ** 5**
 1  3  -1 [-3  5  3] 6  7       **5**
 1  3  -1  -3 [5  3  6] 7       **6**
 1  3  -1  -3  5 [3  6  7]      **7**

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

## Approach-
The most important part of this problem is to know [[Deque]] and how it can be used in this approach.

When consuming an element
If the element being consumed is less than the element at the back of the deque, just push it to the back.
But if it's greater, then pop from the back of the deque until either the deque is empty or the element to be consumed is less than the back of the deque. And then push it to the back.

While leaving element behind with the tail, if the element being left behind is the same as the element at the front, pop from the front of the deque and then move the tail ahead.

### Code 

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        deque<int> dq;
        
        // add the first k elements to the deque        
        for(int i = 0; i < k; i++) {
            if(!dq.empty()) {
                if(nums[i] <= nums[dq.back()]) dq.push_back(i);
                else {
                    while(!dq.empty() && nums[i] > nums[dq.back()]) dq.pop_back();
                    dq.push_back(i);
                }
            }
            else dq.push_back(i);
        }

        vector<int> res;
        int t = 0, h = k - 1;

        while(h + 1 < n) {
            res.push_back(nums[dq.front()]); // print the element at the front of dq

            // move tail ahead
            if(nums[t] == nums[dq.front()]) dq.pop_front();
            t++;

            // move head ahead
            h++;
            while(!dq.empty() && nums[h] > nums[dq.back()]) dq.pop_back();
            dq.push_back(h);
        }
        res.push_back(nums[dq.front()]);
        
        return res;
    }
};
```
