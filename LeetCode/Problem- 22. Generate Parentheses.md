Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3
**Output:** ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

**Input:** n = 1
**Output:** ["()"]

**Constraints:**

- `1 <= n <= 8`

## Approach 1- [[Backtracking]]
One way to solve this problem would be to backtrack using recursion. We mainly use 3 variables, count of open (number of brackets that have been opened), count of closed (number of brackets that have been closed) and a string ans, which will give us the current string under consideration. 
At every step, we check if this string is valid with the following conditions-
- Is the count of the number of brackets closed greater than the number of brackets opened? If yes, return
- Does either of the count have a majority (instead of equal halves)? If yes, return 
- Is the length of open + close = limit of string (2n)? If yes, append to ans vector and return 

Then after that if the function is still running, it means some brackets can be added. So we add one child node to add an opening bracket, and one child node to add a closing bracket. 

And then the same steps from the top are followed.

### Code 
```
class Solution {

public:

vector<string> parentheses;

  

void generate(int open, int close, string str, int limit) {

if(close > open || open == (limit / 2) + 1 || close == (limit / 2) + 1) return;

if(open+close == limit) {

parentheses.push_back(str);

return;

}

generate(open+1, close, str + "(", limit);

generate(open, close+1, str + ")", limit);

}

  

vector<string> generateParenthesis(int n) {

generate(1, 0, "(", 2 * n);

return parentheses;

}

};
```


