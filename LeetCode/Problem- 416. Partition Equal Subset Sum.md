Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false`otherwise.

**Example 1:**

**Input:** nums = [1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

**Example 2:**

**Input:** nums = [1,2,3,5]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

## Approach 1- 2D top down DP
This is the approach I came up with completely on my own but it apparently sucks compared to other people's runtime. But hey, it's still [[progress]].

So my approach was to first find the number we need to search for. If the sum of the entire array is odd, we can never get a successful partition. But if it's even, that number can be found by just dividing the sum by 2.

Then, we basically start from the right of the array to see if we can pick or not pick elements such that the sum (num) we want can be achieved by the picked elements. 

The time complexity of this code is O(n * target) which is the same as the optimal solutions given by neetcode but doing this bottom up or with a 1D array would make the runtime much better.

### Code 
```cpp
class Solution {
public:
    bool rec(vector<int> &nums, int idx, int curr, vector<vector<int>> &dp) {
        if(curr < 0 || idx == -1 && curr > 0) return false;

        if(curr == 0) return true;

        if(dp[idx][curr] != -1) return dp[idx][curr];

        bool ans = rec(nums, idx - 1, curr, dp) || rec(nums, idx - 1, curr - nums[idx], dp);

        return dp[idx][curr] = ans;
    }

    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n + 1, vector<int> (1e5 + 1, -1));

        int sum = 0;
        for(int i = 0; i < n; i++) sum += nums[i]; // get the total sum

        if(sum % 2) return false;
        int num = sum / 2; // get the num to find

        return rec(nums, n - 1, num, dp);
    }
};
```

## Approach 2- 1D Bottom Up Dp
This approach isn't the most intuitive but it's extremely efficient.
Basically we iterate from 0 to n-1 asking the question- what sums can we achieve using upto the ith element.

For every ith element, we iterate from target to 0 to see what sums can be achieved using the formula `dp[j] = dp[j] || dp[j-nums[i]]`
Which means the current target is achievable if-
1. It has already been achieved by previous elements before
2. OR target-nums[i] element has been achieved before so adding nums[i] to that element would help us achieve the current target.

In the end, if dp[target] is true, then we can achieve a successful partition 

### Code 
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for(int i = 0; i < nums.size(); i++) sum += nums[i];
        if(sum % 2) return false;

        int target = sum / 2;
        vector<bool> dp(target + 1, false);
        dp[0] = true;
        
        for(int i = 0; i < nums.size(); i++) {
            for(int j = target; j >= nums[i]; j--) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }

        return dp[target];
    }
};
```

