Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Dynamic Programming]], [[DFS]]

In this [[DSA]] problem, you are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = [2], amount = 3
**Output:** -1

**Example 3:**

**Input:** coins = [1], amount = 0
**Output:** 0

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

## Approach 1- Top-Down DFS with DP 
It took me a while to come up with the idea for the solution and even then, I wasn't getting the implementation right. But I'm sure I'll be able to do it next time if any problem like this comes up. It's actually not that difficult. Just like all recursion problems, visualizing it and getting everything right is the main task as even the tiniest of mistakes will mess up the entire answer. 

We want to find the minimum coins we need to go from the amount given to 0. For that, we perform a [[DFS]] from the amount where the branches will the the amount - coins[i]. We store the result in the DP array so that if we've already seen it before, we can return the result in O(1).

### Code 
``` cpp
class Solution {

public:

vector<int> dp;

  

Solution() : dp(1e4 + 7, -1) {} // init the dp array with -1

  

int dfs(int amount, vector<int> &coins) {

if(amount == 0) return 0; // no coins to operate on

if(dp[amount] != -1) return dp[amount]; // already found the soln for this before

  

int minCoins = INT_MAX;

  

// iterate through all the coins and return the value of minCoin + 1

for(int c: coins) {

if(amount - c >= 0) { // if coin isn't greater than amount

int res = dfs(amount - c, coins);

if(res != INT_MAX) minCoins = min(minCoins, res + 1); // if it's possible to reduce with current coins, update minCoins

}

}

  

dp[amount] = minCoins; // update dp array

return minCoins; // return result

}

  

int coinChange(vector<int>& coins, int amount) {

int res = dfs(amount, coins);

return (res == INT_MAX ? -1 : res); // if it's possible to reduce with current coins, return answer

}

};
```

## Approach 2- Bottom Up
This approach was much more simple to understand as well as code. 
Essentially, we will iterate from 1 to amount and try to find out the minimum value of dp[i] for each i. 
The reason this is simpler is 
1. No need to to handle base cases of any sort since no recursion. 
2. Just have to update dp if value of coin <= i.

This is also a bit faster as the number of computations we have to do goes down since after a limit, we will have seen most of the encountered values so calculation will be done in O[1].
### Code 
``` cpp
class Solution {

public:

int coinChange(vector<int>& coins, int amount) {

vector<int> dp(amount + 1, amount + 1); // init dp array

dp[0] = 0; // dp of 0 will always be 0 here

  

for(int i = 1; i <= amount; i++) { // iterate from 1 to amount

for(int c: coins) { // iterate through all coins

if(c <= i) { // if coin <= i

dp[i] = min(dp[i], 1 + dp[i - c]); // update dp

}

}

}

  

return (dp[amount] == amount + 1 ? -1 : dp[amount]); // if we found an answer, return the ans. Else return -1

}

};
```