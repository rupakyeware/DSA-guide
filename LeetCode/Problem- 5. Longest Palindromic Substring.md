Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Dynamic Programming]]

In this [[DSA]] problem, given a string `s`, return the longest palindromic substring in `s`.

**Example 1:**

**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**

**Input:** s = "cbbd"
**Output:** "bb"

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

## Approach 1- Using 2 pointers 
There are 2 types of palindromes that a substring can be-
1. odd length
2. even length
While checking for odd length, we initialize l and r to a single index and then expand outwards to check if the entire substring is a palindrome (the way we check if the entire string is a palindrome or not)

While checking for even length, we initialize l to i and r to i + 1 and do the same thing.

### Code 
``` cpp
class Solution {
public:
    int resLen = 0;
    string res = "";
    
    void findPalindrome(int l, int r, int n, string s) {
        while(l >= 0 && r < n && s[l] == s[r]) {
                if(r - l + 1 > resLen) {
                    resLen = r - l + 1;
                    res = s.substr(l, resLen);
                }
            r++;
            l--;
        }
    }
    
    string longestPalindrome(string s) { 
        int n = s.size();
        
        for(int i = 0; i < n; i++) {
            // odd palindrome
            findPalindrome(i, i, n, s);
            
            // even palindrome
            findPalindrome(i, i+1, n, s); 
        }

        return res;
    }
};
```

## Approach 2- Using [[Dynamic Programming]]

This approach is less efficient that the previous 2 pointers approach.
Here we basically want to start from the end of the string and move towards the beginning while checking if the right side of i has an palindromic substrings.
To make this process efficient (a little bit but not really) we store the bounds, as in the start and end of the palindromic substrings in a 2D DP array so that when checking for substrings we can just check if i and j are same and the inner string which is `dp[i+1][j-1]` is a palindrome too.
### Code 
``` cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        int resLen = 0;
        int resIdx = 0;
    
        vector<vector<bool>> dp(n, vector<bool> (n));

        for(int i = n - 1; i >= 0; i--) { // iterate start of substring
            for(int j = i; j < n; j++) { // iterate end of substring
                if(s[i] == s[j] && ((j - i <= 2) || (dp[i+1][j-1]))) { // check if ss is a palindrome
                    dp[i][j] = true; // set current bounds of ss as true 
                    if(resLen < j - i + 1) {
                        resLen = j - i + 1;
                        resIdx = i;
                    }
                }
            }
        }

        return s.substr(resIdx, resLen);
    }
};
```