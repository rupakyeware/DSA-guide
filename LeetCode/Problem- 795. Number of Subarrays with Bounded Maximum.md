Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Greedy]], [[Subarrays]], [[Bounded Maximum]]

In this [[DSA]] problem, given an integer array `nums` and two integers `left` and `right`, return _the number of contiguous non-empty **subarrays** such that the value of the maximum array element in that subarray is in the range_ `[left, right]`.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,1,4,3], left = 2, right = 3
**Output:** 3
**Explanation:** There are three subarrays that meet the requirements: [2], [2, 1], [3].

**Example 2:**

**Input:** nums = [2,9,2,5,6], left = 2, right = 8
**Output:** 7

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `0 <= left <= right <= 109`

## Approach 
I wasn't able to come up with this approach myself. I had to look at the solutions. But it's a very nice and efficient approach.

The key logic here is that 
`no. of subarrs within bounds = no. of subarrs with bound(r) - no. of subarrs with bound(l-1)`

And to achieve that, we will use the getCount function. It's a very efficient way to deal with [[Bounded Maximum]] problems like this one.

### Code 
```cpp
class Solution {
public:
    int getCount(vector<int> &nums, int bound) {
        int count = 0;
        int ans = 0;

        for(int &n: nums) {
            if(n <= bound) count++;
            else count = 0;
            ans += count;
        }

        return ans;
    }

    int numSubarrayBoundedMax(vector<int>& nums, int left, int right) {
        return getCount(nums, right) - getCount(nums, left-1);
    }
};
```