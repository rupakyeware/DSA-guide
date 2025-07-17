Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]], [[String]]

In this [[DSA]] problem, given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

**Example 1:**

**Input:** s = "bcabc"
**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"
**Output:** "acdb"

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

## Approach 
The core logic of this problem is to find a way to make sure the lexicographical order of the answer is maintained. For that, we need to know when we can pop the char at the top of the stack. We can pop it if the current character is lexicographically smaller than it and the char at the top appears later on in the string too. 

So for that, I made a [[Frequency Map]]. First, I was using the find() function in STL but chatgpt suggested using this to reduce the time complexity. 

### Code 
```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        string st;
        vector<bool> vis(26, 0); // characters we have visited so far
        vector<int> frq(26, 0); // frq of remaining chars
        for(char c: s) frq[c-'a']++;

        for(int i = 0; i < s.size(); i++) {
            frq[s[i]-'a']--; // reduce frq of current char
            if(!vis[s[i]-'a']) { // if this char isn't in the stack
                while(!st.empty() && // while the top of stack > curr char and we will see it later, pop it
                    st.back() > s[i] &&
                    frq[st.back()-'a'] > 0) 
                {
                    vis[st.back()-'a'] = 0; // mark char to pop as unvisited
                    st.pop_back(); // pop it
                }
                st.push_back(s[i]); // push curr char into stack
                vis[s[i]-'a'] = 1; // mark curr char as visited
            } 
        }

        return st;
    }
};
```