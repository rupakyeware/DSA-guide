Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Backtracking]], [[DFS]]

In this [[DSA]] problem, given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return _all possible palindrome partitioning of_ `s`.

**Example 1:**

**Input:** s = "aab"
**Output:** `[["a","a","b"],["aa","b"]]`

**Example 2:**

**Input:** s = "a"
**Output:** `[["a"]]`

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

## Approach 
We want to perform a [[DFS]] on all the possible places where we can partition if the partition is a palindrome. For that, we'll just use a for loop starting from the beginning index to the end of string and if the substring from beginning index to i is a palindrome, we recursively call the dfs function with the beginning index as i + 1.

### Code 
``` cpp
class Solution {

public:

vector<vector<string>> ans;

vector<string> part;

  

vector<vector<string>> partition(string s) {

dfs(s, 0); // try all partitions starting from 0

return ans;

}

  

void dfs(string &s, int idx) {

if(idx == s.size()) { // we have tried all possible partitions in current branch

ans.push_back(part);

return;

}

  

for(int i = idx; i < s.size(); i++) { // try remaining partitions

if(isPalindrome(s, idx, i)) { // if current partition is a palindrome

part.push_back(s.substr(idx, i - idx + 1));

dfs(s, i + 1); // try next partitions

part.pop_back();

}

}

}

  

// check if substring is a partition

bool isPalindrome(string &s, int l, int r) {

while(l < r) {

if(s[l] != s[r]) return false;

l++;

r--;

}

  

return true;

}

};
```