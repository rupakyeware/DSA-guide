Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Dynamic Programming]], [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, you are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example 1:**

**Input:** nums = [3,1,5,8]
**Output:** 167
**Explanation:**
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167

**Example 2:**

**Input:** nums = [1,5]
**Output:** 10

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `0 <= nums[i] <= 100`

## Approach 
The secret to ==form 4== questions is to think of what will happen in the last state. In this particular question, the last state will be 1 * nums[i] * 1 since it'll be the last element remaining and multiplying with out of bounds element means * 1 (as given in the question).
Then, for each L,R- we iterate from L to R trying to keep the ith element as the last element. 
The transition would be
`dp(L,R) -> dp(L, i-1) + (L-1 * nums[i] * R+1) + dp(i+1, R)`
which is basically 
`dp(L,R) -> best way to handle left partition + score if current element is kept last + best way to handle right partition`

This is a nice question.

### Code 
```cpp
class Solution {
public:
    int interval(int L, int R, vector<int> &nums, vector<vector<int>> &dp) {
        if(L > R) return 0;

        if(dp[L][R] != -1) return dp[L][R];

        int maxi = INT_MIN;

        for(int i = L; i <= R; i++) {
            int curr_score = interval(L, i-1, nums, dp) + interval(i+1, R, nums, dp) + (nums[L-1] * nums[i] * nums[R+1]);
            maxi = max(maxi, curr_score);
        }

        return dp[L][R] = maxi;
    }

    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 1);
        nums.push_back(1);

        vector<vector<int>> dp(nums.size() + 1, vector<int> (nums.size() + 1, -1));

        return interval(1, n, nums, dp);
    }
};
```