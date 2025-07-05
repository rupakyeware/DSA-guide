Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Array]], [[Kadane's Algorithm]]

In this [[DSA]] problem, given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,3,-2,4]
**Output:** 6
**Explanation:** [2,3] has the largest product 6.

**Example 2:**

**Input:** nums = [-2,0,-1]
**Output:** 0
**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `-10 <= nums[i] <= 10`
- The product of any subarray of `nums` is **guaranteed** to fit in a **32-bit** integer.

## Approach 1- Using prefix and suffix multiplication 
We will maintain 3 variables- prefix, suffix and max.
The prefix will go on finding the prefix multiplication and if at any point it becomes 0, it will reset itself to 1.
Same with suffix but for suffix multiplication.
The max will maintain the maximum values so far from prefix and suffix.

This felt a little [[Unintuitive]] to me but definitely better than [[Kadane's Algorithm]].

### Code 
``` cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        
        int prefix = 1;
        int suffix = 1;
        int maxi = INT_MIN;

        for(int i = 0; i < n; i++) {
            if(prefix == 0) prefix = 1;
            if(suffix == 0) suffix = 1;

            prefix *= nums[i];
            suffix *= nums[n-i-1];

            maxi = max(maxi, max(prefix, suffix));
        }

        return maxi;
    }
};
```

## Approach 2- [[Kadane's Algorithm]]