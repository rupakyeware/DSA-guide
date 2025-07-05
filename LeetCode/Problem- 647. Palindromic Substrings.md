Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Dynamic Programming]], [[2 pointers]]

In this [[DSA]] problem, given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"
**Output:** 3
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"
**Output:** 6
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

## Approach 1- [[Dynamic Programming]]
The approach is almost exactly the same as the [[Dynamic Programming]] programming approach in [[Problem- 5. Longest Palindromic Substring]]. The only difference is instead of checking if the current palindromic substring is the longest, we just increment the total count of palindromic substrings found till now.

### Code 
``` cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n + 1, vector<bool> (n + 1, false));
        int count = 0;
        
        for(int i = n - 1; i >= 0; i--) {
            for(int j = i; j < n; j++) {
                if(s[i] == s[j] && ((j - i <= 2) || (dp[i+1][j-1]))) {
                    count++;
                    dp[i][j] = true;
                }
            }
        }

        return count;
    }
};
```

## Approach 2- Using [[2 pointers]]
Same as the above approach, this approach is almost entirely similar to the same previous problem mentioned above and the current approach is also different in the same way as the above.

### Code 
``` cpp
class Solution {
public:
    int isPalindrome(int l, int r, int &n, string &s) {
        int count = 0;
        
        while(l >= 0 && r < n && s[l] == s[r]) {
            count++;
            l--;
            r++;
        }

        return count;
    }
    
    int countSubstrings(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n + 1, vector<bool> (n + 1, false));
        int count = 0;
        
        for(int i = 0; i < n; i++) {
            // check if odd palindrome
            count += isPalindrome(i, i, n, s);

            // check if even palindrome
            count += isPalindrome(i, i+1, n, s);
        }

        return count;
    }
};
```