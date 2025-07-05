Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Binary Search]], [[Answer Space]], [[Sweeping check]], [[Minimization]]
### Description
In this [[DSA]] problem, you are given N distinct points on the number line in a sorted array A. You can place at most KK more points on the line (integer coordinates only). You have to make the maximum separation between any two consecutive points in the final configuration as minimum as possible. Output this minimal value.

Note: You can place the points anywhere you like, but you cannot place more than one point at the same position on the line.

### Approach used

[[Binary Search]] on [[Answer Space]] with [[Sweeping check]] by trying to check if a particular maximum difference can be achieved. 

To calculate the points needed between ```v[x]``` and ```v[x+1]``` we use the formula
``` points_needed = (gap + mid - 1) / mid - 1```

Why? Just doing ``` gap / mid``` may works in conditions where the numerator is not perfectly divisible by the denominator. But if it is, then it may give the wrong answer. To fix this, we force the numerator to give us an answer exactly 1 greater than the correct answer and then we subtract 1.

### Code
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

bool check(vector<int> &v, int n, int k, int mid){

    int points_used = 0;

    for(int i = 0; i < n - 1; i++) {

        int gap = v[i+1] - v[i];

        // To calculate the points needed, the formula (gap / mid) will work for imperfect divisions

        // But not for perfect divisions

        // So we add (mid - 1) to the numerator and subtract one to make it an imperfect division

        // Now we'll always get 1 more point than required and then we subtract 1 to get the correct output

        int points_needed = (gap + mid - 1) / mid - 1;

        points_used += points_needed;

        if(points_used > k) return false;

        // cout << "At " << v[i] << " the total points are " << points_used << endl;

    }

  

    return true;

}

  

void solve() {

    int n, k; cin >> n >> k;

    vector<int> v(n);

    for(int i = 0; i < n; i++) cin >> v[i];

  

    int lo = 1, hi = v[n-1]-v[0], ans = -1;

    while(lo <= hi) {

        int mid = lo + (hi - lo) / 2;

        if(check(v, n, k, mid) == 1) {

            // cout << "mid as " << mid << " worked!" << endl;

            hi = mid - 1;

            ans = mid;

        }

        else {

            // cout << "mid as " << mid << " didnt work!" << endl;

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
