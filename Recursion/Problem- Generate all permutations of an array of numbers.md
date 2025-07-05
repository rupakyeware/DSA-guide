Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Recursion]]

Given an array of integers `nums` that may contain **duplicates**, return **all possible unique permutations** in **any order**.

A **permutation** of an array is an arrangement of its elements. Two permutations are considered different if there is at least one index `i` such that `nums1[i] != nums2[i]`.

You must return a list of permutations where **each permutation is unique**.

### **Example 1**

**Input:**

`nums = [1, 1, 2]`

**Output:**

`[[1,1,2], [1,2,1], [2,1,1]]`

## Approach 
This was a tricky problem. But it helped strengthen my understanding of permutations. 

Essentially, for a particular level in a permutation, it can be swapped with all the elements ahead of it and form permutations. This applies for all levels in the array. 
So we'll just recursively call the function for each level while swapping the value at a particular level with all the values ahead of it. 

`Note: We aren't swapping with the values behind as those paths have already been or will be explored`

### Code 
``` cpp
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

set<vector<int>> ans;

  

void addAns(vector<int> &v) {

ans.insert(v);

}

  

void recur(vector<int> v, int level, int n) {

if(level == n) { // base case: current permutation is finished

addAns(v);

return;

}

  

// swap and call func again for each level after current level

for(int i = level; i < n; i++) {

swap(v[level], v[i]); // swap element at current index with ith element after current index

recur(v, level + 1, n); // call func again for next level

}

}

  

void solve() {

int n; cin >> n;

vector<int> v(n);

for(int i = 0; i < n; i++) cin >> v[i];

  

recur(v, 0, n);

  

for(vector<int> v: ans) {

for(int i: v) cout << i << " ";

cout << endl;

}

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