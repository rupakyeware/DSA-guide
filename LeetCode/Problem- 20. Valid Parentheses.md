Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Stack]], [[Unordered Map]]

In this [[DSA]] problem, given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "([])"

**Output:** true

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

## Approach- 
I had learnt the approach to solve this in [[AlgoZenith]]. It's actually very simple. We want to assign every type of opening bracket a positive integer value and it's corresponding closing bracket the additive inverse of the same value.

So for eg. `{` = 1 and `}` = -1
Then, we maintain a stack and if we encounter an opening bracket, we push it to the stack. Else, if we encounter a closing bracket, we first check if the stack is empty. If it is, it means that we're trying to close a bracket that doesn't exist. But if a value exists in the stack, we check if the sum of the value of the closing bracket encountered and the value at the top of the stack is 0. If it is, it means we closed a valid opening bracket and we can pop an element from the stack. If it isn't, it means we tried to close a bracket that isn't of the same type as the one last opened, so we return false.  

After we finish iterating through the string, if the stack is not empty, it means we haven't closed all the brackets that were opened, so we return false. Else if it's empty, we return true;

### Code 
```
class Solution {

public:

bool isValid(string s) {

unordered_map<char, int> umap = {

{'(' , 1},

{')' , -1},

{'{' , 2},

{'}' , -2},

{'[' , 3},

{']' , -3}

};

  

int len = s.size();

stack<int> st;

  

for(int i = 0; i < len; i++) {

if(umap[s[i]] > 0) st.push(umap[s[i]]);

else {

if(st.empty()) return false;

if(umap[s[i]] + st.top() != 0) return false;

st.pop();

}

}

  

if(!st.empty()) return false;

return true;

}

};
```