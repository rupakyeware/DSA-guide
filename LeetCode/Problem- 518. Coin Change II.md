Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]]

In this [[DSA]] problem, you are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

**Input:** amount = 5, coins = [1,2,5]
**Output:** 4
**Explanation:** there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

**Example 2:**

**Input:** amount = 3, coins = [2]
**Output:** 0
**Explanation:** the amount of 3 cannot be made up just with coins of 2.

**Example 3:**

**Input:** amount = 10, coins = [10]
**Output:** 1

**Constraints:**

- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- All the values of `coins` are **unique**.
- `0 <= amount <= 5000`

## Approach 1- 1D DP 
We will create a 1D dp of unsigned ints since the answer may be up to 32 bits and a normal int (signed) has only 31 bits. We will initialize it with all 0s.

Since the number of ways to make an amount of 0 with 0 coins is 1, we will set dp[0] = 1.

Let's say n = coins.size()

Then we will iterate from coin[i] to amount in the dp array while performing this operation
`dp[i] = dp[i - coin] + dp[i]`

What we are doing is figuring out the number of ways each amount can be formed with the ith coin.

For every amount, we have 2 options, either pick the coin, or don't pick it.
`dp[i-coin]` gives the count picking the current coin while `dp[i]` gives the count if we don't pick the current coin.

### Code 
``` cpp
class Solution {

public:

int change(int amount, vector<int>& coins) {

vector<unsigned int> dp(amount + 1, 0); // answer may be up to 32 bits, so we need an unsigned int

dp[0] = 1; // number of ways to form 0 with 0 coins in 1

  

for(int c: coins) {

for(int i = c; i <= amount; i++) {

// no of ways to form the amount using the current coin-

// ways to form it picking the current coin

// +

// ways to form it not picking the current coin

dp[i] = dp[i - c] + dp[i];

}

}

  

return dp[amount];

}

};
```

## Approach 2- 2D dp using form 2
This is more intuitive but has a slower runtime. 
We will use form 2 but along with that we'll also borrow the pick/not pick approach from form 1.

If we choose to not pick an element, we will go to level-1 with the same amount.
But  if we choose to pick an element, we will go to the same level again (since the same coin can be picked again) with amount-coins[level] as the new amount.

That's the only change.

### Code 
```cpp
class Solution {
public:
    int rec(int level, int amount, vector<int> &coins, vector<vector<int>> &dp) {
        if(amount < 0) return 0;
        
        if(level == -1) {
            if(amount == 0) return 1;
            else return 0;
        }

        // dp check
        if(dp[level][amount] != -1) return dp[level][amount];

        // transition
        int notPick = rec(level-1, amount, coins, dp);
        int pick  = rec(level, amount-coins[level], coins, dp);

        // save and return
        return dp[level][amount] = notPick + pick;
    }
    
    int change(int amount, vector<int>& coins) {
        int n = coins.size();
        vector<vector<int>> dp(n + 1, vector<int> (amount + 1, -1));
        return rec(n-1, amount, coins, dp);
    }
};
```