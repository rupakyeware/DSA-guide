Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[DFS]], [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**

**Input:** nums = [2,7,9,3,1]
**Output:** 12
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

## Approach 
I rewrote the approach for this one as I came up with a better and cleaner code.
I completely forgot that I had solved this problem and I'd also completely forgotten dp.
I studied [[Problem- 01 Knapsack]] in the morning and I was able to come up with and write the solution to this problem in 11 minutes. I actually shocked myself because I wrote that I had struggled a little bit with this problem in my last approach. That's some good [[progress]].

As for the approach itself, it's very similar to knapsack but just 1D since we only need to know what's the maximum money we can get if we choose to rob the current house. And if not picking this house gives us a better result, we do that.

### Code 
``` cpp
class Solution {
public:
    int dfs(vector<int> &nums, vector<int> &dp, int house, int n) {
        if(house >= n) return 0; // no more houses left to rob
        if(dp[house] != -1) return dp[house]; // already know how much money we can get starting from current house

        int notPick = dfs(nums, dp, house + 1, n); // money if we don't rob this house
        int pick = nums[house] + dfs(nums, dp, house + 2, n); // money if we do rob this house

        return dp[house] = max(notPick, pick);
    }
    
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, -1);

        return dfs(nums, dp, 0, n);
    }
};
```
This code is a lot smaller and cleaner than the code I wrote in my previous approach. 