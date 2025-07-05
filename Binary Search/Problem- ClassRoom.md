Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Binary Search]], [[Answer Space]]

In this [[DSA]] problem, Vivek has built a new classroom with N seats. The seats are located along a straight line at positions x1,x2…..xN.  
Vivek has to assign seats to K students such that a seat can be assigned to at most 1 student and the minimum distance between any two students is as large as possible. Find the largest minimum distance possible.

### Approach
We have to maximize the minimum distance between the seats while placing k students and then return that distance. It's a classic binary search on answer space problem. We set low to the min(distance between all pairs of points) in the array and max to the distance between the first and last point. 
One trick to quickly analyse what lo and hi should be set to initially is to think about it like this

lo -> what is the absolute minimum answer that can be computed regardless of the input.
hi -> what is the absolute maximum answer that can be computer regardless of the input.
ans -> what is that default answer that should be returned if there are no 0s in the output space.

This trick should help understand what the hi and lo should be without having to spend too much time.

For the check function, we set the position of the first student to be on the first bench by default and we keep a track of the bench where the last student was placed.
Then, for all pairs after that, we check if the distance to the bench where the last student was placed is >= mid. If it is, then we can place a student here. Else, move to the next bench and check again.

### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

bool check(vector<int> &v, int n, int k, int mid) {

    int studentsPlaced = 1; // 1st student definitely be placed on the first available bench

    int lastStudentPlacedAt = v[0]; // store the position of the bench where the last student was placed

  

    for(int i = 1; i < n; i++) {

        if(v[i] - lastStudentPlacedAt < mid) continue; // if we can't place a student here, move to next bench

        studentsPlaced++; // else place a student

        lastStudentPlacedAt = v[i]; // store the position of the bench

        // if(studentsPlaced >= k) return true; // check if k or more students have been placed

    }

  

    return studentsPlaced >= k; // cannot place k students with a min distance of mid

}

  

void solve() {

    int n, k; cin >> n >> k;

    int minDistance = INT_MAX;

  

    vector<int> v(n);

    for(int i = 0; i < n; i++) {

        cin >> v[i];

        if(i) minDistance = min(v[i] - v[i-1], minDistance);

    }

    sort(v.begin(), v.end());

  

    // absolute minimum distance for any k can be the min(distance between two points) for all consecutive pair of points

    // absolute max distance for any k can be the distance between the first and last point in the array

    // we should always get an ans so initialize it to anything

    int lo = minDistance, hi = v[n-1] - v[0], ans = 0;

    while(lo <= hi) {

        int mid = lo + (hi - lo) / 2;

        if(check(v, n, k, mid) == 1) {

            ans = mid;

            lo = mid + 1;

        }

        else {

            hi = mid - 1;

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
