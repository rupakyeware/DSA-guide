Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Sliding Window]]

In this [[DSA]] problem, given an array of positive integers `nums` and a positive integer `target`, return _the **minimal length** of a_ _subarray_ _whose sum is greater than or equal to_ `target`. If there is no such subarray, return `0` instead.

**Example 1:**

**Input:** target = 7, nums = [2,3,1,2,4,3]
**Output:** 2
**Explanation:** The subarray [4,3] has the minimal length under the problem constraint.

**Example 2:**

**Input:** target = 4, nums = [1,4,4]
**Output:** 1

**Example 3:**

**Input:** target = 11, nums = [1,1,1,1,1,1,1,1]
**Output:** 0

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

## Approach 
I was able to come up with the solution for this very quickly (although I thought it was for multiplication, not sum. But the approach was the same) but the coding took me so long it's actually embarrassing. I need to practise [[Sliding Window]] and [[2 pointers]] more to get the hang of it. I kept getting heap buffer overflow or missing the last element. 

Basically we move the head with a for loop and the head will keep adding elements.
While the curr is >= target, we first update ans if needed, and then subtract nums[t] from curr and move the tail ahead.

### Code 
``` cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        int ans = INT_MAX;
        int curr = 0;

        int t = 0;
        for(int h = 0; h < n; h++) {
            curr += nums[h];
            while(curr >= target) {
                ans = min(ans, h - t + 1);
                curr -= nums[t];
                t++;
            }
        }

        return (ans == INT_MAX ? 0 : ans);
    }
};
```