Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Recursion]], [[Dynamic Programming]], [[DFS]]

In this [[DSA]] problem, You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**

**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

## Approach
This is a basic DP question. We use DFS to find the steps needed. Every time we find the steps needed for a particular node, we update the dp array with the value at index of step. Then, next time we encounter that value again, we don't have to expand that sub tree again. We can just get the value in O(1) from the dp array.

### Code 
``` cpp
class Solution {

public:

vector<int> stepsTakenPreviously;

  

Solution() : stepsTakenPreviously(46, -1) {};

  

int climbStairs(int n) {

if(stepsTakenPreviously[n] != -1) return stepsTakenPreviously[n];

  

if(n == 1) {

stepsTakenPreviously[n] = 1;

return 1;

}

  

if(n == 2) {

stepsTakenPreviously[n] = 2;

return 2;

}

  

int oneStep = climbStairs(n-1);

int twoSteps = climbStairs(n-2);

int totalSteps = oneStep + twoSteps;

stepsTakenPreviously[n] = totalSteps;

  

return totalSteps;

}

};
```