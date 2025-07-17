Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

## Approach 1- DP with form 2
I thought of solving this using form 2 of dp where we will try to get the maximum subarray ending at every index. That will help us decide if we should choose or not choose to expand the subarray backwards.

### Code 
``` cpp
class Solution {
public:
    int rec(vector<int> &nums, vector<int> &dp, int idx) {
        // cout << "at idx " << idx << endl;
        if(idx == 0) return nums[idx];
        
        if(dp[idx] != -1) return dp[idx];

        int ans = nums[idx];
        // cout << "ending subarr at " << idx << " we can get sum as " << ans << endl;
        int pick = nums[idx] + rec(nums, dp, idx-1);
        // cout << "extending subarr backwards from " << idx << " we can get sum as " << pick << endl;
        ans = max(ans, pick);
        // cout << "ans at " << idx << " = " << ans << endl; 

        return dp[idx] = ans;
    }

    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size(), -1);
        int maxi = INT_MIN;

        for(int i = nums.size()-1; i >= 0; i--) {
            // cout << "trying subarr ending at " << i << endl;
            maxi = max(maxi, rec(nums, dp, i));
        }

        return maxi;
    }
};
```


## Approach 2- [[Kadane's Algorithm]]
This was actually much more intuitive than I thought it would be. We just make a decision. If the prefix sum (currSum) is less than 0, then it means it's contributing adversely to our subarray so we will stop considering it i.e. set it to 0.
Then we add the value of the current number to currSum regardless of the previous condition. And then we set maxSum as the max of its current value and the value of currSum.

### Code 
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // kadane's algorithm
        int currSum = 0;
        int maxSum = INT_MIN;

        for(int i = 0; i < nums.size(); i++) {
            // if currSum before this is < 0, stop considering this subarray
            // and start considering a new subarray from the current idx
            if(currSum < 0) { 
                currSum = 0;
            }
            currSum += nums[i]; // add value of current num to the consideration
            maxSum = max(maxSum, currSum); // update max if needed
        }

        return maxSum;
    }
};
```