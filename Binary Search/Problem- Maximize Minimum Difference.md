Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]], [[Greedy]]

In this [[DSA]] problem,

you are given an array of N integers arr You need to find a subsequence of size K such that the minimum absolute difference between any two numbers in it is maximum.

Print this largest minimum difference.

##### Input Format

The first line of the input contains one integer T - the number of test cases. Then T test cases follow.

The first line of each test case contains two space-separated integers - N and K.

The second line of each test case contains NN space-separated integers arr1,arr2,...,arrNarr1​,arr2​,...,arrN​.

##### Output Format

For each test case, print the output on a new line.

##### Constraints

1≤T≤1031≤T≤103 2≤K≤N≤1052≤K≤N≤105 1≤arri≤1091≤arri​≤109 It's guaranteed that the sum of NN over all test cases ≤5×105≤5×105.

##### Sample Input 1

1 4 3 1 2 3 5

##### Sample Output 1

2

##### Sample Input 2

1 3 2 5 17 11

##### Sample Output 2

12

##### Note

In Sample Input 1, Possible subsequences of size 3 are `[1,2,3`], `[1,2,5]`, `[1,3,5] `and `[2,3,5]`.

For `[1,3,5]` possible differences are (∣1–3∣=2)(∣1–3∣=2), (∣3–5∣=2)(∣3–5∣=2) and (∣1–5∣=4)(∣1–5∣=4), Min(2,2,4)=2Min(2,2,4)=2.

For the remaining subsequences, the minimum difference comes out to be 1. Hence, the maximum of all minimum differences is 2.

## Approach 
We will perform a binary search on the [[Answer Space]]. The answer space will be the values of maximum minimum difference. And for the boolean check function, we will use a greedy approach where we will first pick the first element and see if it the difference between it and any other element is at least mid. If it is, then we will set picked as the current element and keep doing the same for the entire array. If the number of count of numbers we picked are >= mid, then this particular iteration passes so we try for a higher maximum minimum difference value. 

For the greedy approach to work properly without over increasing the time complexity, we will have to first sort the array.

### Code 
``` cpp
#include<bits/stdc++.h>
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#define int long long
#define endl '\n'
using namespace std;

vector<int> arr;

bool minDiff(int mid, int k) {
    int picked = arr[0];
    int count = 1;

    for(int i = 1; i < arr.size(); i++) {
        if(arr[i]-picked >= mid) {
            count++;
            picked = arr[i];
        }
    }

    return count >= k;
}


void solve() {
    int n, k; cin >> n >> k;
    arr.resize(n);
    int mini = INT_MAX, maxi = INT_MIN; 
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
        mini = min(mini, arr[i]);
        maxi = max(maxi, arr[i]);
    }

    sort(arr.begin(), arr.end());

    int lo = 0, hi = maxi-mini, ans = 0;
    while(lo <= hi) {
        int mid = lo + (hi-lo) / 2;
        if(minDiff(mid, k)) {
            ans = mid;
            lo = mid+1;
        }
        else {
            hi = mid-1;
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