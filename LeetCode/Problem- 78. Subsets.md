Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Backtracking]], [[Recursion]], [[Subsets]]

In this [[DSA]] problem, given an integer array `nums` of **unique** elements, return _all possible__subsets_ _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:**` [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

**Example 2:**

**Input:** nums = [0]
**Output:** `[[],[0]]`

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.

## Approach - 
When it comes to subsets, remember that for every element, we have two choices. To pick it, or to not pick it. And for every choice we make, we give the same 2 choices again for the next element until we reach the last element and get all the possible subsets.

That's the exact same logic I've carried out here where I create 2 branches for every branch until I have traversed all the elements in the input array and at the leaf nodes, I push the current vector to the answer vector. 

### Code 
``` cpp 
class Solution {

public:

vector<vector<int>> ans;

  

void findSubset(vector<int>& nums, int n, int currElem, vector<int> curr) {

if(currElem >= n) {

ans.push_back(curr);

return;

}

  

// Don't choose curr element

findSubset(nums, n, currElem + 1, curr);

  

// Choose curr element

curr.push_back(nums[currElem]);

findSubset(nums, n, currElem + 1, curr);

  

}

  

vector<vector<int>> subsets(vector<int>& nums) {

int n = nums.size();

vector<int> curr = {};

findSubset(nums, n, 0, curr);

  

return ans;

}

};
```