Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[2 pointers]]

In this [[DSA]] problem, you are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 5
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transactions are done and the max profit = 0.

## Approach-
We will use 2 pointers for this, l, which will point to the buying price, and r, which will point to the selling price. In the beginning, we set l to the 0th index and r to the 1st. The current profit can be computed as prices[r] - prices[l]
If the curr profit is > max profit, we set the max profit as curr profit. Then we try to search for a better selling price by doing r++;
But if it's less than 0, it means our buying price is too high and we need to search for a better buying price so we'll do l++;
We keep on doing this until r has traversed the entire array.

### Code-
```
class Solution {

public:

int maxProfit(vector<int>& prices) {

int n = prices.size();

int l = 0, r = 1;

  

int max_profit = 0;

while(r < n) {

int curr_profit = prices[r] - prices[l];

if(curr_profit < 0) l++;

else {

max_profit = max(max_profit, curr_profit);

r++;

}

}

  

return max_profit;

}

};
```