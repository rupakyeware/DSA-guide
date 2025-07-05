Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Prefix Sum]], [[Binary Search]], [[Sliding Window]]

In this [[DSA]] problem, you are given a binary array of length N. The score of an array is the length of the longest continuous subsegment consisting of only 1. Find the maximum score possible if you can change at most K elements of the array.

Basically, you are given an array like
1 0 1 1 0
You have to find the maximum possible score if k (no. of 0s you can flip at max) = 1
Ans would be 4 as if you flip the 1st 0, a subarray of 4 1s can be created so the max score is 4.

## Approach 1- Binary Search
Time Complexity: O(nlog(n))

This is a binary search on [[Answer Space]] with a [[Sweeping check]] kind of problem. We try to find if a score of 'mid' can be attained where the size of the window will be mid. You slide the window through a prefix array of the number of 0s in the vector and try to find the maximum possible score.
For a given window, the score would be mid - no. of zeroes in window + zeroes we can add.
If the score meets our mid, we return true and try for a higher score. Else we return false and try for a lower score.

### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

// Will return true if an subarray of score 'size' can be made by flipping <= k 0s

bool check(vector<int> prefix_0s, int n, int k, int size) {

    int start = 1, end = size;  // Set the initial start and end of the window

    while(end <= n) {

        int zeroes_in_window = prefix_0s[end] - prefix_0s[start - 1]; // Find number of zeroes in the current window

        int score = size - zeroes_in_window + k; // Score of current window is no. of elems - zeroes in window + zeroes we can add

        if(score >= size) return true; // If this score meets ask of mid return true

        start++; end++;// Else move the window to the next position

    }

    return false; // Could not find a window that meets the requirement of the mid

}

  

void solve() {

    int n, k; cin >> n >> k;

    vector<int> v(n);

    for(int i = 0; i < n; i++) cin >> v[i];

  

    // Create a prefix array to find the number of 0s within any range quickly

    vector<int> prefix_0s(n + 1, 0);

    for(int i = 1; i <= n; i++) {

        prefix_0s[i] = prefix_0s[i-1];

        if(!v[i-1]) prefix_0s[i]++;

    }

  

    // A window containing atleast k 0s can definitely be made so lo = k

    // A window with at max n elements with all 1s can be obtained so hi = n

    // A minimum value of k will always be found and ans will be replaced so initialization isn't needed.

    int lo = k, hi = n, ans;

    while(lo <= hi) {

        int mid = lo + (hi - lo) / 2;

        if(check(prefix_0s, n, k, mid) == 1) { // If mid score is attainable, try for a higher score

            ans = mid;

            lo = mid + 1; // Move lower boundary to mid + 1

        }

        else { // Try for a lower score

            hi = mid - 1; // Move upper boundary to mid - 1

        }

    }

  

    cout << ans << endl;

}

  

signed main() {

    ios_base::sync_with_stdio(false);

    cin.tie(NULL); cout.tie(NULL);

  

    int _t;

    cin >> _t;

  

    while(_t--) solve();

    return 0;

}
```

## Approach 2- [[2 pointers]]
Time complexity: O(n)
This is a very simple yet amazing approach to solve this problem quickly. 
We use 2 pointers, a tail and a head. The head will check if it can 'consume' some more elements while keeping the condition of <= k zeroes in mind. Once it can't consume any more, it will compare the ans with the maxAns and set it accordingly. Then we will increment the tail and check if the element we just passed was a 0. If it was, then decrement the number of zeroes we are tracking in the array.

```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n, k; cin >> n >> k;

vector<int> v(n);

for(int i = 0; i < n; i++) cin >> v[i];

  

int h = -1, t = 0, ans = 0;

int zeroes = 0;

while(h < n) {

// check if we can consume the next element

while(h + 1 < n && ((1 - v[h+1]) + zeroes <= k)) {

h++;

if(v[h] == 0) zeroes++;

}

  

ans = max(ans, h - t + 1); // set ans as curr length of subarr if it's > ans

  

if(t > h) { // if h < t, it may get stuck. So move tail one step ahead and place head right behind it

t++;

h = t - 1;

}

else { // check if tail current points to a zero. If yes, decrement zeroes and move tail ahead

if(v[t] == 0) zeroes--;

t++;

}

}

  

cout << ans << endl;

}

  

signed main() {

ios_base::sync_with_stdio(false);

cin.tie(NULL); cout.tie(NULL);

  

int _t;

cin >> _t;

  

while(_t--) solve();

return 0;

}
```