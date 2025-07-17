Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Greedy]]

In this [[DSA]] problem, given an integer array `nums`, return `true` _if there exists a triple of indices_ `(i, j, k)` _such that_ `i < j < k` _and_ `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.

**Example 1:**

**Input:** nums = [1,2,3,4,5]
**Output:** true
**Explanation:** Any triplet where i < j < k is valid.

**Example 2:**

**Input:** nums = [5,4,3,2,1]
**Output:** false
**Explanation:** No triplet exists.

**Example 3:**

**Input:** nums = [2,1,5,0,4,6]
**Output:** true
**Explanation:** The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.

**Constraints:**

- `1 <= nums.length <= 5 * 105`
- `-231 <= nums[i] <= 231 - 1`

**Follow up:** Could you implement a solution that runs in `O(n)` time complexity and `O(1)` space complexity?

## Approach 
A brute force algorithm would be just to have  3 nested for loops and check if nums[i] < nums[j] < nums[k]. If it is, we return true;

But a much better solution is to take a greedy approach here.
We'll maintain two variables, smallest and second smallest (s_smallest). They will track the first and second smallest numbers we have seen up to the ith point in the array. Then, if we get a number which is greater than both, first and second smallest, we have our triplet.

===NOTE: first and second smallest don't represent the actual values but more like intervals. So if smallest = -2 and s_smallest = 4, it means that we have elements from -2 to 4 present in the array such that they can form a triplet if we get a value greater than both of them. ===
This is what I struggled to grasp for a bit until it hit me.

### Code 
```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int smallest = INT_MAX;
        int s_smallest = INT_MAX;

        for(int n: nums) {
            if(n <= smallest) smallest = n;
            else if(n <= s_smallest) s_smallest = n;
            else return true;
        }

        return false;
    }
};
```

