Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Recursion]], [[Dynamic Programming]], [[DFS]]

In this [[DSA]] problem, you are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return _the minimum cost to reach the top of the floor_.

**Example 1:**

**Input:** cost = [10,15,20]
**Output:** 15
**Explanation:** You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.

**Example 2:**

**Input:** cost = [1,100,1,1,1,100,1,1,100,1]
**Output:** 6
**Explanation:** You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.

**Constraints:**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`

## Approach 
We want to know how much it would cost us to reach the top if we step on a particular step.
And we want to know that fast.

So we will create a dp array of the size of the cost array as that's the number of steps that we have to pass.

Then we perform [[DFS]] to find the cost of each step and store that in the dp array. 

Now when we encounter that step again, we already know how much it would cost so then we can choose whether to step on it or skip it,

### Code 
``` cpp
class Solution {

public:

int takeStep(int step, vector<int> &dp, vector<int> &cost, int n) {

if(step == n) {

return 0;

}

  

if(dp[step] != -1) return dp[step];

  

int minCost = INT_MAX;

  

// take 1 step

int cost_1 = takeStep(step + 1, dp, cost, n);

minCost = min(minCost, cost_1);

  

// take 2 steps

if(step + 2 <= n) {

int cost_2 = takeStep(step + 2, dp, cost, n);

minCost = min(minCost, cost_2);

}

  

// store final cost of node to dp and return it

int nodeCost = minCost + cost[step];

dp[step] = nodeCost;

  

return nodeCost;

}

  

int minCostClimbingStairs(vector<int>& cost) {

vector<int> dp(cost.size(), -1);

int cost0 = takeStep(0, dp, cost, cost.size());

int cost1 = takeStep(1, dp, cost, cost.size());

  

return min(cost0, cost1);

}

};
```