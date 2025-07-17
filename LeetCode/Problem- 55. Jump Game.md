Source: [[Leetcode]]
Difficulty: [[Medium]]
Topcis: [[Dynamic Programming]], [[Recursion]], [[Greedy]]

In this [[DSA]] problem, you are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** true
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [3,2,1,0,4]
**Output:** false
**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

## Approach 
The [[Dynamic Programming]] programming approach is pretty straightforward so I won't write that. This is the most optimal solution with O(n) time and O(1) space complexity. 
This is a very intuitive approach too. If we want to reach the 5th step, for example, and the 4th step can get us there. Our goal is now updated to reach the 4 step. And let's say the 1st step can get us there, so our goal now becomes step 1. And if, from step 2, we can jump a maximum of 3 steps, that means we can reach the first step too, and in turn reach the 5th step, thereby achieving our goal.

### Code 
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int goal = nums.size()-1;

        for(int i = nums.size()-2; i >= 0; i--) {
            if(i + nums[i] >= goal) goal = i;
        }

        return goal == 0;
    }
};
```