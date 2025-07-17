Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[String]]

In this [[DSA]] problem, given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**

**Input:** digits = "23"
**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example 2:**

**Input:** digits = ""
**Output:** []

**Example 3:**

**Input:** digits = "2"
**Output:** ["a","b","c"]

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

## Approach 
This was pretty simple but I needed a small hint from chatgpt to recognize that this was a recursion problem. 
Essentially, we just pick one character associated with the current number at a time and then call the function for the next index. If we reach idx = n, then we push our current string to the ans vector.

### Code 
```cpp
class Solution {
public:
    vector<string> ans;
    string curr = "";
    vector<string> chars;

    void makeCombination(int idx, string &digits) {
        if(idx == digits.size()) {
            ans.push_back(curr);
            return;
        }

        string &currIdx = chars[digits[idx]-'2'];

        for(char c: currIdx) {
            curr.push_back(c);
            cout << c << endl;
            makeCombination(idx+1, digits);
            curr.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        chars = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        if(digits.size() == 0) return {};

        makeCombination(0, digits);
        return ans;
    }
};
```