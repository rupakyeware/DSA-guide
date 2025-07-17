Source: [[GFG]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]]. [[Frequency Vector]]

In this [[DSA]] problem, you are given a string **s** consisting only lowercase alphabets and an integer **k**. Your task is to find the **length** of the **longest substring** that contains exactly **k** distinct characters.

**Note :** If no such substring exists, return **-1**. 

**Examples:**

**Input:** s = "aabacbebebe", k = 3
**Output:** 7
**Explanation**: The longest substring with exactly 3 distinct characters is "cbebebe", which includes 'c', 'b', and 'e'.

**Input**: s = "aaaa", k = 2
**Output:** -1
**Explanation**: There's no substring with 2 distinct characters.  

**Input:** s = "aabaaab", k = 2
**Output:** 7
**Explanation**: The entire string "aabaaab" has exactly 2 unique characters 'a' and 'b', making it the longest valid substring.

**Constraints:**  
1 ≤ s.size() ≤ 105  
1 ≤ k ≤ 26

## Approach 
We will use a simple sliding window approach with a head and tail pointer and at each iteration of the head, we will check if consuming the current character is going to increase our count more than k. If it is, we will keep moving the tail ahead while decrementing the frequency of those characters. If the tail decremented the frequency of a character and its count became 0, it means we removed a unique character from the substring, so we will reduce the count by 1.

Then in the same iteration, after the tail's loop, we will consume the current character and increment count if necessary. The important part of this problem is that we **only** want those substrings that have **exactly** k distinct characters. No more, no less. So if count == k after the consuming the current character, we set the value of longest.

### Code 
``` cpp
class Solution {
  public:
    int longestKSubstr(string &s, int k) {
        // code here
        int n = s.size();
        int t = 0;
        int count = 0;
        vector<int> frq(26, 0);
        
        int longest = -1;
        for(int h = 0; h < n; h++) {
            char &c = s[h];
            while(frq[c-'a'] == 0 && count == k) {
                frq[s[t]-'a']--;
                if(frq[s[t]-'a'] == 0) count--;
                t++;
            }
            if(frq[c-'a'] == 0) count++;
            frq[c-'a']++;
            if(count==k) longest = max(longest, h-t+1);
        }
        
        return longest;
    }
};
```