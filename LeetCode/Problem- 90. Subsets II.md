Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given an integer array `nums` that may contain duplicates, return _all possible__subsets_ _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,2]
**Output:**` [[],[1],[1,2],[1,2,2],[2],[2,2]]`

**Example 2:**

**Input:** nums = [0]
**Output:** `[[],[0]]`

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

## Approach 
This problem is similar to [[Problem- 78. Subsets]]. The approach is similar to it but I also borrowed from the approach used in [[Problem- 40. Combination Sum II]] on how we skip elements if we don't want to consider them, which is necessary if we want to prune the subtrees that might give us duplicate solutions. 

==Note: In problems like these that can be solved with [[Recursion]] but need unique solutions- sorting the array first is a critical step which will let us skip duplicates effectively. ==

### Code 
``` cpp
class Solution {

public:

vector<vector<int>> subsets;

  

void pick(vector<int> &nums, vector<int> curr, int idx, int n) {

if(idx == n) { // we have reached the end of the nums list

subsets.push_back(curr); // push current subset to subsets

return;

}

  

// pick the current index

curr.push_back(nums[idx]);

pick(nums, curr, idx + 1, n);

  

curr.pop_back(); // delete the element we just considered

  

// don't pick the current index

int ptr = idx + 1;

while(ptr < n && nums[ptr] == nums[ptr-1]) ptr++;

pick(nums, curr, ptr, n);

  

}

  

vector<vector<int>> subsetsWithDup(vector<int>& nums) {

sort(nums.begin(), nums.end()); // CRITICAL STEP FOR SUCH PROBLEMS

pick(nums, {}, 0, nums.size()); // start the tree with the 0th element in the list

  

return subsets;

}

};
```