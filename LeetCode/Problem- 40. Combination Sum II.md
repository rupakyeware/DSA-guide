Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
```
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
```
[
[1,2,2],
[5]
]
```

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

## Approach 
The approach is a little similar to [[Problem- 39. Combination Sum]] where we will recursively call the function to consider every possible solution.

But here, we aren't allowed to reuse the element at a particular index more than once. The tricky part is when we have to handle duplicate elements. Because we can use an element as many times as it occurs in the array. But if we just decide to skip an element and then land on a duplicate element, it might product duplicate results.

To fix this, when skipping or not considering elements, we want to skip all occurrences of that element. We can do this easily by just sorting the array once in the beginning and then calling the function with the next non duplicate element. 

==Note: In problems like these that can be solved with [[Recursion]] but need unique solutions- sorting the array first is a critical step which will let us skip duplicates effectively. ==

That's the only difference.


### Code 
``` cpp
class Solution {

public:

vector<vector<int>> combinations;

  

void combine(vector<int> &candidates, int curr, vector<int> currVec, int sum, int target) {

if(sum == target) { // found an answer

combinations.push_back(currVec);

return;

}

  

if(curr == candidates.size() || sum > target) return; // no need to traverse this subtree

  

// don't consider the current element

int nextElement = curr + 1;

while(nextElement < candidates.size() && candidates[nextElement] == candidates[nextElement-1]) nextElement++; // find the next non-duplicate element

combine(candidates, nextElement, currVec, sum, target);

  

// consider the current element

int newSum = sum + candidates[curr];

currVec.push_back(candidates[curr]);

combine(candidates, curr + 1, currVec, newSum, target);

}

  

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {

sort(candidates.begin(), candidates.end()); // sorting the array is extremely necessary in this problem in order to properly skip duplicate elements

combine(candidates, 0, {}, 0, target);

return combinations;

}

};
```