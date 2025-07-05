Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]]

In this [[DSA]] problem, given a string `s`, find the length of the **longest** **substring** without duplicate characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

## Approach
I was able to solve this pretty easily without any help. 
Just need to maintain a map where the frequencies of the elements in the current substring under consideration are stored. We determine the substring by using two pointers, l, which will be pointing to the 0th index in the beginning, and r, which will be pointing to the 1st index in the beginning. The r will consume elements. And if the frequency of the rth element is > 1, we move the l forward while decrementing the count of the lth character. Spaces are also taken into consideration with this approach.

## Code 
```
class Solution {

public:

int lengthOfLongestSubstring(string s) {

if (s.size() == 1) return 1; // Won't have any repeating characters anyway

unordered_map<char, int> umap;

int l = 0, r = 1, n = s.size();

// Initialize the values of the map with the lth and rth values in the string

umap[s[l]]++;

umap[s[r]]++;

int longest = 0;

while(r < n) {

while(umap[s[r]] > 1) { // If the element just consumed is repeated before, move the l ahead until that element is removed

umap[s[l]]--;

l++;

}

// Now a valid window is being considered

// Calcualate current length of window and update longest window if needed

int curr_len = r - l + 1;

longest = max(longest, curr_len);

r++; umap[s[r]]++; // Consume the next element

}

  

return longest;

}

};
```
