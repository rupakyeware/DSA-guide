Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, given two strings `s1` and `s2`, you need to find the minimum length of a diff string that represents the transformation from `s1` to `s2` using only **insert** and **delete**operations.

A diff string is a sequence of operations where:

- Characters that remain unchanged are represented as themselves
- Characters that need to be deleted from `s1` are represented as `-X` (where X is the character)
- Characters that need to be inserted into `s1` are represented as `+X` (where X is the character)

Each element in the diff string (whether it's a plain character, `-X`, or `+X`) counts as **one unit** toward the total length.

Return an array of two elements:

1. The minimum possible length of the diff string
2. One valid diff string of minimum length

## Examples

### Example 1:

```
Input: s1 = "AATU", s2 = "ATUA"
Output: [6, "A-A+T-T+U+A"]
Explanation: 
- Keep 'A' (position 0)
- Delete 'A' (position 1) → -A
- Insert 'T' → +T  
- Delete 'T' (position 2) → -T
- Insert 'U' → +U
- Insert 'A' → +A
Total length: 6 units
```

### Example 2:

```
Input: s1 = "ABC", s2 = "AC"
Output: [3, "A-BC"]
Explanation:
- Keep 'A'
- Delete 'B' → -B
- Keep 'C'
Total length: 3 units
```

### Example 3:

```
Input: s1 = "XY", s2 = "XYZ"
Output: [3, "XY+Z"]
Explanation:
- Keep 'X'
- Keep 'Y'
- Insert 'Z' → +Z
Total length: 3 units
```

### Example 4:

```
Input: s1 = "ABCD", s2 = "EFGH"
Output: [8, "-A-B-C-D+E+F+G+H"]
Explanation:
- Delete all characters from s1
- Insert all characters from s2
Total length: 8 units
```

### Example 5:

```
Input: s1 = "SAME", s2 = "SAME"
Output: [4, "SAME"]
Explanation:
- All characters match, no operations needed
Total length: 4 units
```

## Constraints

- `1 <= s1.length, s2.length <= 500`
- `s1` and `s2` consist of only uppercase English letters
- If multiple diff strings of minimum length exist, return any one of them

## Approach 
We will use ==form 3== of dp where we will iterate each of the strings using 2 pointers. 
First, we will just calculate the length of the minimum diff string and after that, we will retrace the path we took to form the string itself.

If moving i ahead is better than moving j ahead, it means we are removing an element from A. But if moving j ahead is better, then it means we are adding an element to A.
But if both are the same, we keep the element as it is.

### Code 
```
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

string A, B; 
int sA, sB;

int movePointers(int i, int j, vector<vector<int>> &dp) {
    // pruning
    if(i == sA && j == sB) return 0; // both strings have been exhausted

    // base case 
    if(i == sA) return sB - j; // only A has been exhausted, so add remaining chars from B to A
    if(j == sB) return sA - i; // only B has been exhausted, so remove remaining chars from A

    // cache check 
    if(dp[i][j] != INT_MAX) return dp[i][j];

    int ans = 0;
    // transition 
    if(A[i] == B[j]) ans = 1 + movePointers(i+1, j+1, dp);
    else {
        ans = min(1 + movePointers(i+1, j, dp), 1 + movePointers(i, j+1, dp));
    }

    // save and return 
    return dp[i][j] = ans;
}

string retrace(vector<vector<int>> &dp) {
    string ans = "";
    int i = 0, j = 0; // start checking from the beginning of both strings

    while(i < sA || j < sB) { // either of the strings is yet to be traversed
        if(i == sA && j < sB) { // A has been traversed but B is still remaining so add all as +ve from B
            while(j < sB) {
                ans += "+" + string(1, B[j]);
                j++;
            }
        }
        else if(j == sB && i < sA) { // B has been traversed but A is still remaining so remove all as -ve from A
            while(i < sA) {
                ans += "-" + string(1, A[i]);
                i++;
            }
        }
        else if(A[i] == B[j]) { // characters are matching so add just the element
            ans += string(1, A[i]);
            i++; j++;
        }
        else { // both strings are yet to be traversed fully
            int moveA = (i + 1 <= sA ? dp[i+1][j] : INT_MAX); // if we move A ahead (-ve)
            int moveB = (j + 1 <= sB ? dp[i][j+1] : INT_MAX); // if we move B ahead (+ve)

            if(moveA < moveB) { // moving A ahead gives a smaller string
                ans += "-" + string(1, A[i]);
                i++;
            }
            else { // moving B ahead gives a smaller string
                ans += "+" + string(1, B[j]);
                j++;
            }
        }
    }

    return ans;
}

void solve() {
    cin >> A >> B;
    sA = A.size(); sB = B.size();
    vector<vector<int>> dp(sA + 1, vector<int> (sB + 1, INT_MAX));
    cout <<  movePointers(0, 0, dp) << endl;
    cout << retrace(dp) << endl;
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