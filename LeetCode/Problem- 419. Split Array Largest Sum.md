Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Binary Search]] on [[Answer Space]], [[Greedy]]

In this [[DSA]] problem, given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return _the minimized largest sum of the split_.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [7,2,5,10,8], k = 2
**Output:** 18
**Explanation:** There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 2
**Output:** 9
**Explanation:** There are four ways to split nums into two subarrays.
The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 106`
- `1 <= k <= min(50, nums.length)`

## Approach 
This is a textbook binary search on answer space problem. The answer space here is the minimum maximum sum we can get where we will perform a binary search from the lower element of the array to the sum of the entire array. 

In the isPossible function, I'm checking if it's possible to take the current element in the current subarray, if it is, I increase the value of curr. Else if the value is less than mid, I create a new subarray and set the sum as the current value. But if the value is greater than mid, then we can't ever achieve the current mid, so we return false.

### Code 
```cpp
class Solution {
public:
    bool isPossible(vector<int> &nums, int &k, int &mid) {
        int subarrs = 1;
        int curr = 0;

        for(int n: nums) {
            if(n > mid) return false;
            if(curr + n > mid) {
                subarrs++;
                curr = 0;
            }
            curr += n;
        }

        return subarrs <= k;
    }

    int splitArray(vector<int>& nums, int k) {
        int sum = 0;
        int mini = INT_MAX;

        for(int n: nums) {
            sum += n;
            mini = min(mini, n);
        }       

        int lo = mini, hi = sum, ans = sum;
        while(lo <= hi) {
            int mid = lo + (hi-lo)/2;

            if(isPossible(nums, k, mid)) {
                ans = mid;
                hi = mid-1;
            }
            else {
                lo = mid+1;
            }
        }

        return ans;
    }
};
```