Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given an integer array `nums`, return _the length of the longest **strictly increasing**_ _**subsequence**_.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]
**Output:** 4

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1

**Constraints:**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))`time complexity?

## Approach 1- 2D Top Down DP using form 1
This is the approach I came up with. I was able to think of the solution and write a recursive solution completely by myself but I needed some help from chatgpt to make it dp.
Basically I'm keeping a track of the previously selected index and the current index.
We have two choices, either don't pic k the current element and try to form a sequence without or pick it and try to form a sequence with it. But we can only pick the current element if it's greater than the previously selected element.

### Code 

```cpp
class Solution {
public:
    int rec(vector<int> &nums, int idx, int prev_idx, vector<vector<int>> &dp) {
        if(idx == nums.size()) return 0;

        if(dp[idx][prev_idx + 1] != -1) return dp[idx][prev_idx + 1];

        // not pick
        int notPick = rec(nums, idx + 1, prev_idx, dp);

        // pick
        int pick = 0;
        if(prev_idx == -1 || nums[idx] > nums[prev_idx])
            pick = 1 + rec(nums, idx + 1, idx, dp);
        

        return dp[idx][prev_idx + 1] = max(notPick, pick);
    }

    int lengthOfLIS(vector<int>& nums) {
        vector<vector<int>> dp(nums.size() + 1, vector<int> (nums.size() + 1, -1));
        return rec(nums, 0, -1, dp);
    }
};
```

Although this solution has an n^2 time complexity, it's not efficient because of the overhead and extra space being taken up.

## Approach 2- 1D Top Down DP
Here, we will basically try to find the longest increasing subsequence for each start. So the driver will call the recursive function for every i in nums and then the recursive function will iterate from i to n and call itself again for every jth element where nums[i] < nums[j]. And the DP will store the max LIS that can be formed for an ith element in nums. Which means we only need a 1D DP array this time.

### Code 
```cpp
class Solution {
public:
    int findLIS(vector<int> &nums, int idx, vector<int> &dp) {
        if(dp[idx] != -1) return dp[idx];

        int maxi = 1;
        
        for(int j = idx + 1; j < nums.size(); j++) {
            if(nums[idx] < nums[j]) {
                maxi = max(maxi, 1 + findLIS(nums, j, dp));
            }
        }

        return dp[idx] = maxi;
    }

    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size() + 1, -1);
        int maxi = 1;

        for(int i = 0; i < nums.size(); i++) {
            maxi = max(maxi, findLIS(nums, i, dp));
        }

        return maxi;
    }
};
```

## Approach 3- 1D Top Down DP using form 2
Here, we only ask one question- ==What's the longest subsequence we can form having the ith index in the last place?==
We do this for n elements and return the max.

To answer the question itself,
we recursively call the same again and add 1 (to include the current element) only if the element we're comparing the ith element to is less than the current element in value (only then can an increasing subsequence be formed). We do this for all elements from 0 to i and return the maximum.

The intuition isn't as difficult as the previous approach.

### Code 
```cpp
class Solution {
public:
    int rec(vector<int> &nums, int idx, vector<int> &dp) {
        // pruning 
        if(idx < 0 || idx >= nums.size()) return 0;

        // base case
        if(idx == 0) return 1;

        // cache check 
        if(dp[idx] != -1) return dp[idx];

        // transition
        int maxi = 1;

        for(int i = 0; i < idx; i++) {
            if(nums[idx] > nums[i]) maxi = max(maxi, 1 + rec(nums, i, dp));
        } 

        return dp[idx] = maxi;
    }   

    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        int maxi = 1;
        vector<int> dp(n + 1, -1);
        for(int i = 0; i < n; i++) {
            maxi = max(maxi, rec(nums, i, dp));
        }

        return maxi;
    }
};
```