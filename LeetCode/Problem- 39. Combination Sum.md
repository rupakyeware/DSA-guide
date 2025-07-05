Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** `[[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

## Approach 
We basically want to find all the viable options for each candidate. And the options for each candidate will be the elements greater than or equal to the last element taken into consideration. 

If the sum of the elements under consideration is equal to the target, we found an answer. If it's greater, then we prune this sub tree.

But if it's lesser than the target, we search for more options.

### Code 
``` cpp
class Solution {

public:

vector<vector<int>> combinations;

  

bool check(int sum, int num, int target) { // returns true if current element can be considered

return sum + num <= target;

}

  

void tryCombination(vector<int> curr, int sum, int target, vector<int> &candidates, int start) {

int size = curr.size();

if(sum == target) combinations.push_back(curr);

  

// sum is lesser than target so try adding more elements

for(int i = start; i < candidates.size(); i++) {

if(candidates[i] >= curr[size-1] && check(sum, candidates[i], target)) { // if current element can be considered

vector<int> newCurr = curr;

newCurr.push_back(candidates[i]); // add it to list of elements

tryCombination(newCurr, sum + candidates[i], target, candidates, i); // call function again with new elements

}

}

}

  

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {

sort(candidates.begin(), candidates.end());

int size = candidates.size();

for(int i = 0; i < size; i++) {

if(check(0, candidates[i], target)) tryCombination({candidates[i]}, candidates[i], target, candidates, i);

}

  

return combinations;

}

};
```