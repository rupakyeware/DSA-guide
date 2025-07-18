Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]]

In this [[DSA]] problem, given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

**Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
**Output:** 6
**Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output:** 10
**Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

## Approach 
This is basically the same approach used in [[Problem- 1493. Longest Subarray of 1's After Deleting One Element]] but instead of a boolean has0, here we have an int has0 value. And the 0s currently in the array can be flipped so we'll consider the total distance of h and t as the length of our [[Subarrays|subarray]].

### Code 
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int longest = 0, c0 = 0;
        int t = 0;
        for(int h = 0; h < nums.size(); h++) {
            if(nums[h] == 0) c0++;
            while(c0 > k) {
                if(nums[t] == 0) c0--;
                t++;
            }
            longest = max(longest, h-t+1);
        }

        return longest;
    }
};
```