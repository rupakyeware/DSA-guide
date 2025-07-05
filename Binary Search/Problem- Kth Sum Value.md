
Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Binary Search]], [[Answer Space]], [[Sweeping check]]

##### Description
In this [[DSA]] problem, you are given two arrays _A_ of size _N_ and _B_ of size _M_ and an integer _K_. Create a new array _C_ of size N*M consisting of A[i]+B[j] for 1≤i≤N, 1≤j≤M. Find the Kth smallest element in the array C.

Basically, if you are given an input of k as 3, find the 3rd smallest element in the sorted array C.

### Approach
This question can be solved using a [[Binary Search]] on the [[Answer Space]] with [[Sweeping check]]. However, the key thing to find out here is what the answer space is.
All binary search problems, from easy to hard, that I've solved till now have been solved by bringing them down to 1 specific pattern. And that's lower_bound.
```0 0 0 0 1 1 1 1```
In the above array, the answer is the 1st 1, which is at index 4.
Hence, we have to form such an array with it and return the first 1.

But this question is a bit tricky.
The tricky to solve it is this formula
*For an element mid in the sorted array C, find the number of elements that are less than or equal to mid. If that number is greater than or equal to k, return true*

We need to perform two binary searches in this question.
One, to create an array like the one given above.
The second, to find the number of element <= mid from the array C.
This is because finding the no. of elements in n^2 will cause a TLE. So we iterate through the n array while performing a binary search on the m array and updating the count.

### Code

```
// In this question, we perform a binary search on the answer space. We have to check if the number of elements less than mid

// are less than or equal to k. The first '1' will be our answer.

  

// But to check the number of elements less than or equal to k, we will have to perform a second binary search.

// We will iterate through the n vector and perform a binary search on the m vector to find the number of values less than equal to k.

  

#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

// Returns true if mid <= target, else false

bool checkIfLTEk(int mid, int target) {

    if(mid <= target) return true;

    else return false;

}

  

// Performs a 2nd binary search that returns true if number of vals from C that are <= mid are >= k

bool numOfVals(vector<int> &vn, vector<int> &vm, int n, int m, int mid, int k) {

    int total = 0;

    for(int i = 0; i < n; i++) {

        // Binary search to find the number of elements <= mid

        int lo = 0, hi = m - 1, ans = -1;

        while(lo <= hi) {

            int mid2 = lo + (hi - lo) / 2;

            if(checkIfLTEk(vn[i] + vm[mid2], mid)) {

                lo = mid2 + 1;

                ans = mid2;

            }

            else {

                hi = mid2 - 1;

            }

        }

        total += (ans + 1);

        if(total >= k) return true;

    }

  

    return false;

}

  

void solve() {

    int n, m, k; cin >> n >> m >> k;

    vector<int> vn(n); vector<int> vm(m);

  

    for(int i = 0; i < n; i++) cin >> vn[i];

    for(int i = 0; i < m; i++) cin >> vm[i];

  

    // Make sure the n vector is smaller than the m vector in size.

    // This is because in the second binary search, we will iterate through the n vector

    if(n > m) {

        swap(n, m);

        swap(vn, vm);

    }

  

    // Sort both the vectors to prepare them for our 2nd binary search

    sort(vn.begin(), vn.end());

    sort(vm.begin(), vm.end());

  

    // Binary search on answer space

    // This will check if number of values LTE mid are >= k

    int lo = vn[0] + vm[0], hi = vn[n-1] + vm[m-1], ans = -1;

    while(lo <= hi) {

        int mid = lo + (hi - lo) / 2;

        if(numOfVals(vn, vm, n, m, mid, k)) {

            hi = mid - 1;

            ans = mid;

        }

        else {

            lo = mid + 1;

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


