Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Sliding Window]], [[2 pointers]]

In this [[DSA]] problem, given a binary array `nums`, you should delete one element from it.

Return _the size of the longest non-empty subarray containing only_ `1`_'s in the resulting array_. Return `0` if there is no such subarray.

**Example 1:**

**Input:** nums = [1,1,0,1]
**Output:** 3
**Explanation:** After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

**Example 2:**

**Input:** nums = [0,1,1,1,0,1,1,0,1]
**Output:** 5
**Explanation:** After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

**Example 3:**

**Input:** nums = [1,1,1]
**Output:** 2
**Explanation:** You must delete one element.

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

## Approach 
This was a classical sliding window approach but I needed to look at the hints to know that. I was trying to solve it using greedy and dp. I guess sometimes the simple ways are the best ways, huh.

So here, we'll keep moving the head ahead and incrementing c if head is pointing at 1. But if it's pointing at a 0, we set has0 to true and then move ahead. But if has0 is already true, then we need to move the tail ahead while removing elements from the back. If it removes a 1, c decreases by 1, but if it removes a 0, has0 becomes false. And then has0 becomes true again because the head currently points at a 0.

### Code 
``` cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int n = nums.size();

        int t = 0;
        int c = 0, longest = 0;
        bool has0 = false;
        for(int h = 0; h < n; h++) {
            if(nums[h] == 0 && !has0) {
                has0 = true;
            }
            else if(nums[h] == 0 && has0) {
                while(has0) {
                    if(nums[t] == 0) {
                        has0 = false;
                    }
                    else {
                        c--;
                    }
                    t++;
                }
                has0 = true;
            }
            else {
                c++;
            }
            longest = max(longest, c);
        }

        return (longest == nums.size() ? longest-1 : longest);
    }
};
```