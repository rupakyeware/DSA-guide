Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]]

In this [[DSA]] problem, given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]

**Example 3:**

**Input:** nums = [], target = 0
**Output:** [-1,-1]

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`

## Approach 

### Code 
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res;
        int n = nums.size();

        int lo = 0, hi = n-1, ans = -1;
        while(lo <= hi) {
            int mid = lo + ((hi-lo) / 2);
            if(nums[mid] == target) {
                ans = mid;
                hi = mid - 1;
            }
            else if(nums[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }

        if(ans < 0) return {-1,-1};
        res.push_back(ans);

        lo = 0, hi = n-1, ans = -1;
        while(lo <= hi) {
            int mid = lo + ((hi-lo) / 2);
            if(nums[mid] == target) {
                ans = mid;
                lo = mid + 1;
            }
            else if(nums[mid] < target) lo = mid + 1;
            else hi = mid - 1;
        }
        res.push_back(ans);

        return res;
    }
};
```