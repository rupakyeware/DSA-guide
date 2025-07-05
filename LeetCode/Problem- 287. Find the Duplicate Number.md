Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Floyd's Tortoise & Hare]], [[Linked List]], [[2 pointers]], [[Unintuitive]]

In this [[DSA]] problem, given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]`inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and using only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**

**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Example 3:**

**Input:** nums = [3,3,3,3,3]
**Output:** 3

This problem would've been easily solved with a hashmap had it not been for the constraints. But because of the constraints, we have to use [[Floyd's Tortoise & Hare]] here. The solution is extremely unintuitive so don't even attempt to deduce how we came to it. Just memorize it.

## Approach- 
Instead of looking at the array as just an array, look at it like a [[Linked List]], where the index i is pointing to the index at value of index i.
Like I said, unintuitive.
Then there are 2 steps to solving it.
1. Use [[Floyd's Tortoise & Hare]] to find where the cycle matches.
2. Initialize a second slow pointer and iterate it until slow1 and slow2 match. Wherever they do, that's our answer.
```
class Solution {

public:

int findDuplicate(vector<int>& nums) {

int slow = 0, fast = 0;

while(true) {

slow = nums[slow];

fast = nums[nums[fast]];

  

if(slow == fast) break;

}

  

int slow2 = 0;

while(true) {

slow = nums[slow];

slow2 = nums[slow2];

if(slow == slow2) return slow;

}

}

};
```