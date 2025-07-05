Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Dynamic Programming]]

In this [[DSA]] problem, given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]
**Output:** true
**Explanation:** Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]
**Output:** true
**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
**Output:** false

**Constraints:**

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are **unique**.

## Approach-
A brute force approach would be to try to match the substring of string s starting from the ith position with each word in wordDict and then if it returns true for any word, recursively call the function again with the new idx starting after the recently matched substring.

To add dp to this, if we reach a point where none of the words are matching, we want to avoid coming to that index again, so we set dp[idx] = false. This will help us avoid having to compare the strings in wordDict again.

### Code 

```cpp
class Solution {
public:
    bool compare(string s, int idx, string w) {
        if(idx + w.size() <= s.size()) {
            return s.substr(idx, w.size()) == w;
        }

        return false;
    }

    bool match(string s, int idx, vector<string> &wordDict, vector<int> &dp) {
        if(idx == s.size()) return true;

        if(dp[idx] != -1) return dp[idx];

        // try to match with all words in dic
        for(string word: wordDict) {
            if(compare(s, idx, word)) {
                // if word is matching, then return true and store result
                if(match(s, idx + word.size(), wordDict, dp)) 
                    return dp[idx] = true;
                // else try remaining words
            }
        }

        // none of the words in dic matched, so return false
        return dp[idx] = false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        vector<int> dp(s.size() + 1, -1);
        return match(s, 0, wordDict, dp);
    }
};
```
