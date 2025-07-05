Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

**Input:** nums = [1,1,1,1,1], target = 3
**Output:** 5
**Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

**Example 2:**

**Input:** nums = [1], target = 1
**Output:** 1

**Constraints:**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `-1000 <= target <= 1000`

## Approach 
I first tried to solve it using ==form 2== but that didn't work as the amount could go to negative some times so then I thought let's just add the amount with the total sum of the array and then store that as the amount in the dp but then going from right to left (form 2) meant that the sum could go outside the bounds of -sum < 0 < sum and it caused a heap buffer overflow.

Instead, neetcode's approach was a tiny bit different where he went from the left to the right (form 1). Everything else, including the transition, was the same. But going from left to right meant that our amount would always be in the bounds.

### Code 
```cpp
class Solution {
public:
    int sum = 0;
    int n;
    int t;

    int rec(int level, int amount, vector<int> &nums, vector<vector<int>> &dp) {
        // pruning

        // base case 
        if(level == n) {
            return amount == t;
        }

        // dp check 
        if(dp[level][amount+sum] != INT_MIN) return dp[level][amount+sum];

        // transition
        int add = rec(level+1, amount+nums[level], nums, dp);
        int sub = rec(level+1, amount-nums[level], nums, dp);

        // save and return
        return dp[level][amount+sum] = add + sub;
    }

    int findTargetSumWays(vector<int>& nums, int target) {
        n = nums.size();
        t = target;
        for(int i = 0; i < n; i++) sum += nums[i];
        vector<vector<int>> dp(n, vector<int> (2*sum+1, INT_MIN));

        return rec(0, 0, nums, dp);
    }
};
```