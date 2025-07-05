Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[2 pointers]], [[Sliding Window]]

Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window**_**_substring_** _of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is **unique**.

**Example 1:**

**Input:** s = "ADOBECODEBANC", t = "ABC"
**Output:** "BANC"
**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**

**Input:** s = "a", t = "a"
**Output:** "a"
**Explanation:** The entire string s is the minimum window.

**Example 3:**

**Input:** s = "a", t = "aa"
**Output:** ""
**Explanation:** Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

## Approach 
My first thought was to use binary search on answer space. It passed 234/268 test cases. But it failed when both s and t were huge. But hey, I was able to think of a bruteforce approach as well a bit of an optimized approach for a hard problem. That's progress. 
Then I watched NeetCode's solution video to understand his linear time solution. It's actually not that hard. I don't know why this is a [[Hard]] problem. 

We'll make 1 [[Unordered Map]] for string s and t each. t's map will keep a count of the frq of its elements. We'll also have 2 variables, have and want. Want will be the size of t's map, while have, initially will be 0. Have will dictate whether the current substring is valid or not. 

Then we want to iterate the string with 2 pointers, l and r. The r will consume an element at every step of the for loop. If we have reached a valid substring (count of elements in t == count of those exact elements in s), then we update the result if necessary and keep popping elements using l until have != need. Then r continues consuming until have == need. Then repeat. Conceptually, it's pretty simple. It's just that reaching this solution intuitively is going to take some practise. 

### Code 
```
class Solution {

public:

string minWindow(string s, string t) {

int n = s.size();

if(t.size() > n) return "";

  

int l = 0;

int ans_idx = -1, ans_len = INT_MAX;

  

unordered_map<char, int> t_map;

for(char c: t) {

t_map[c]++;

}

int have = 0, need = t_map.size();

  

unordered_map<char, int> s_map;

for(int r = 0; r < n; r++) {

char c = s[r];

if(t_map.count(c)) {

s_map[c]++;

if(s_map[c] == t_map[c]) {

have++;

}

}

  

while(have == need) {

int currLen = r - l + 1;

if(currLen < ans_len) {

ans_len = currLen;

ans_idx = l;

}

  

char cl = s[l];

if(t_map.count(cl)) {

s_map[cl]--;

if(s_map[cl] < t_map[cl]) {

have--;

}

}

l++;

}

}

  

return ans_idx > -1 ? s.substr(ans_idx, ans_len) : "";

}

};
```
