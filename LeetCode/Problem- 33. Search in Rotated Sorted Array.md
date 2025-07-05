Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]]

In this [[DSA]] problem, there is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`

## Approach 
I didn't need any help for this problem but it still look me a while. I was able to come up with the solution but the it took me a few tries to get the implementation right for all test cases. At first I was trying to make separate arrays from the pivot. Then I tried to pass the sliced array as a parameter. In the end, what worked was passing the start and end of the search space as parameters and then just running a binary search on that. 

If the target is greater than the last element of the array, it may lie in the rotated part of the array so we pass left and right as 0 and pivot - 1 respectively.

Else we pass pivot and n-1.
### Code 
``` cpp
class Solution {

public:

bool check(int first, int n) { return n < first; }

  

int binser(vector<int> nums, int target, int left, int right) {

int lo = left, hi = right;

while(lo <= hi) {

int mid = lo + (hi - lo) / 2;

if(target < nums[mid]) hi = mid - 1;

else if(target > nums[mid]) lo = mid + 1;

else return mid;

}

return -1;

}

  

int search(vector<int>& nums, int target) {

int n = nums.size();

  

// Find the pivot point of the array

int lo = 0, hi = n-1, ans = 0;

while(lo <= hi) {

int mid = lo + (hi - lo) / 2;

if(check(nums[0], nums[mid])) {

ans = mid;

hi = mid - 1;

}

else lo = mid + 1;

}

  

// If the target may lie in the rotated part

if(target > nums[n - 1]) {

return binser(nums, target, 0, ans - 1);

}

// Else if the may lie in the unrotated part

else {

return binser(nums, target, ans, n - 1);

}

return -1;

}

};
```