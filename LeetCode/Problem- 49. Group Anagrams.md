Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Array]], [[String]]

In this [[DSA]] problem, given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

**Example 1:**

**Input:** strs = `["eat","tea","tan","ate","nat","bat"]`

**Output:** `[["bat"],["nat","tan"],["ate","eat","tea"]]`

**Explanation:**

- There is no string in strs that can be rearranged to form `"bat"`.
- The strings `"nat"` and `"tan"` are anagrams as they can be rearranged to form each other.
- The strings `"ate"`, `"eat"`, and `"tea"` are anagrams as they can be rearranged to form each other.

**Example 2:**

**Input:** strs = `[""]`

**Output:** `[[""]]`

**Example 3:**

**Input:** strs = `["a"]`

**Output:** `[["a"]]`

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

## Approach 1- Using an [[Unordered Map]]
In this approach, we will sort each string and then using that sorted string as a key, we will append it to the respective vector in the map.

### Code 
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        int n = strs.size();
        unordered_map<string, vector<string>> anagrams;

        for(int i = 0; i < n; i++) {
            string sorted = strs[i];
            sort(sorted.begin(), sorted.end());
            anagrams[sorted].push_back(strs[i]);
        }

        vector<vector<string>> res;

        for(auto it: anagrams) {
            res.push_back(it.second);
        }

        return res;
    }
};
```

## Approach 2- Making a key with [[Frequency Map]]
If we want to reduce the time complexity and avoid sorting, we can create a key for each unique anagram by making a frequency map for each string and then adding that to the hash map using that key. 

### Code 
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> grps;

        for(string &s: strs) {
            vector<int> cnt(26, 0);
            for(char c: s) cnt[c-'a']++; // get the count of each char

            // turn the count into a key
            string key = to_string(cnt[0]);
            for(int i = 1; i < 26; i++) {
                key += ',' + to_string(cnt[i]);
            }

            // add the og string to the grp of its key
            grps[key].push_back(s);
        }

        vector<vector<string>> res;

        for(auto &i: grps) {
            res.push_back(i.second);
        }

        return res;
    }
};
```