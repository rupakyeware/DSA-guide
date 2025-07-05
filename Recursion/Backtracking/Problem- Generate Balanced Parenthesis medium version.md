Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Recursion]], [[Backtracking]]

In this [[DSA]] problem, you have given two positive integers _n_ and _k._ Your task is to print all balanced parenthesis of length _n_ in lexicographic order (https://en.wikipedia.org/wiki/Lexicographic_order) with depth **exactly** _k._ 

Balanced parentheses mean that each opening symbol has a corresponding closing symbol and the pairs of parentheses are properly nested.

Note:

1. depth("") = 0.
2. depth('(' + A + ')) = depth(A) + 1, where A is a balanced paranthesis sequence.
3. depth(A + B) = max(depth(A), depth(B)), where A and B are both balanced parenthesis sequence.
4. depth("(") = depth(")") = 0

## Approach 
This problem is similar to [[Problem- 22. Generate Parentheses]] but the difference is that here we only want the answer with depth exactly k.
The solution actually just needed a slight tweak to the approach used in the above mentioned problem. 

I kept a variable called maxDepth which will return the actual depth of the entire string of parentheses and another variable currDepth which maintains the running depth.

So at the end, for a valid answer, the currDepth should be 0, maxDepth should be k, size of string should be n and it should be balanced.

The pruning conditions are mostly going to remain the same except for one added condition where we pure if maxDepth > k.

### Code 
``` cpp
#include <bits/stdc++.h>

#define endl '\n'

using namespace std;

  

vector<string> balanced;

int n, k;

  

void addBracket(int open, int close, string ans, int currDepth, int maxDepth) {

maxDepth = max(maxDepth, currDepth);

if(close > open || open > n/2 || maxDepth > k || (ans.size() == n && maxDepth != k)) return;

  

if(ans.size() == n && currDepth == 0 && maxDepth == k) {

balanced.push_back(ans);

return;

}

  

addBracket(open + 1, close, ans + "(", currDepth + 1, maxDepth);

addBracket(open, close + 1, ans + ")", currDepth - 1, maxDepth);

}

  

void solve() {

cin >> n >> k;

addBracket(1, 0, "(", 1, 0);

for(string s: balanced) cout << s << endl;

}

  

int main() {

std::ios_base::sync_with_stdio(false);

std::cin.tie(nullptr);

solve();

return 0;

}
```