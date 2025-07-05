Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]]

In this [[DSA]] problem, you are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return _an integer that represents the value of the expression_.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Example 1:**

**Input:** tokens = ["2","1","+","3","*"]
**Output:** 9
**Explanation:** ((2 + 1) * 3) = 9

**Example 2:**

**Input:** tokens = ["4","13","5","/","+"]
**Output:** 6
**Explanation:** (4 + (13 / 5)) = 6

**Example 3:**

**Input:** tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
**Output:** 22
**Explanation:** ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

**Constraints:**

- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

## Approach 
This problem was simple. It's just a basic program to evaluate a postfix notation. The only difference was that the values could be negative too. And isdigit() needs char as input. So figuring out how that worked took a moment.

I'm essentially checking if the first or second index in the string are a digit. If they are, then I used stoi() to convert the string to an integer and then push it to the stack. Else I pop 2 of the top most elements in the stack and then evaluate them. Then I push them back into the stack. At the end, only 1 element will be present in the stack. So I return stack.top() as the answer.

```
class Solution {

public:

int evalRPN(vector<string>& tokens) {

stack<int> result;

for(string s: tokens) {

if(isdigit(s[0]) || isdigit(s[1])) {

result.push(stoi(s));

}

else {

int num1 = result.top();

result.pop();

int num2 = result.top();

result.pop();

  

if(s == "+") result.push(num1 + num2);

else if(s == "*") result.push(num1 * num2);

else if(s == "-") result.push(num2 - num1);

else if(s == "/") result.push(num2 / num1);

}

}

return result.top();

}

};
```