Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[String]], [[Array]]

In this [[DSA]] problem, you are given a vector of strings. You are allowed to change any character in these strings. Your task is to return the **minimum number of changes** required so that no two consecutive characters in any string are the same.

##### Input Format

The input consists of:

- An integer nn representing the number of strings in the vector.
- nn lines, where each line contains a string consisting of lowercase English letters only.

##### Output Format

A single integer representing the minimum number of changes required.

##### Constraints

1≤n≤1051≤n≤105 1≤∣si∣≤101≤∣si​∣≤10

##### Sample Input 1

3 abba aabb abc

##### Sample Output 1

3

##### Sample Input 2

2 aaaa bbbb

##### Sample Output 2

4

##### Note

In the first example:

- In the string "abba", change the second 'b' to any other character, say 'c'. The string becomes "abca".
- In the string "aabb", change the second 'a' to any other character, say 'c', and 'b' to 'd'. The string becomes "acdb".
- Total changes: 3. In the second example:
- Change two 'a's in the first string to any other character to avoid consecutive 'a's.
- Similarly, change two 'b's in the second string.

## Approach 
Since we just have to find the minimum number of changes and not actually make the changes in the strings, we can just use this simple logic- `Count the number of consecutive characters while iterating through the string from left to right, and the moment we hit a non consecutive character, increment the count of total by curr/2.`
The reason this works can be seen through an example 
If the string is aaaa, how many replacements do we need? 2 (4/2)
If the string is aaa, how many replacements do we need? 1 (3/2)
So we just use this approach and find the total number of replacements for a string in O(n).

### Code 
``` cpp
#include <bits/stdc++.h>
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

int findReplacements(string s) {
    int n = s.size();
    int totalRepl = 0;
    int currRepl = 1;

    for(int i = 1; i < n; i++) {
        if(s[i] == s[i-1]) currRepl++;
        else {
            totalRepl += (currRepl/2);
            currRepl = 1;
        }
    }

    totalRepl += (currRepl/2);

    return totalRepl;
}

void solve() {
    int n; cin >> n;
    int total = 0;
    while(n--) {
        string s; cin >> s;
        total += findReplacements(s);
    }

    cout << total << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    // cin >> _t;

    while(_t--) solve();
    return 0;
}
```