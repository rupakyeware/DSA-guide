Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Binary Search]], [[Answer Space]], [[Atomic contribution]], [[Minimization]]
### Problem Statement

in this [[DSA]] problem, you are given `N` machines, each taking a specific amount of time to produce a single product. The `i`-th machine takes `v[i]` seconds to produce one product. Your task is to determine the **minimum time required** to produce exactly `T` products using these machines.

#### Constraints

- Each machine can work **independently and simultaneously**.
    
- Machines can produce multiple products over time.
    
- The goal is to minimize the total time required to manufacture `T` products.

### Approach

We can solve this question using [[Binary Search]] on [[Answer Space]] with [[Atomic contribution]].
We will try to see the number of products that can be produced using n machines in mid seconds. We have to find the minimum time needed to produce t products, which is why binary searching on the answer space is an optimal solution.
### Code

```
    #include<bits/stdc++.h>

    #define int long long

    #define endl '\n'

    using namespace std;

  

    // Try to produce t products in mid seconds

    bool check(vector<int> &v, int n, int t, int mid) {

        int products_produced = 0;

  

        for(int i = 0; i < n; i++) {

            products_produced += mid / v[i]; // Calculate total products that can be produced by machine at i in mid seconds

            if(products_produced >= t) return true; // If t or more products have been produced return true;

        }

  

        return false;

    }

  

    void solve() {

        int n, t; cin >> n >> t;

        vector<int> v(n);

        for(int i = 0; i < n; i++) {

            cin >> v[i];

        }

  

        // max time needed to make t products will be t * v[0], if only first machine is given the sole responsibility to make t products

        int lo = 0, hi = t * v[0], ans = -1;

        while(lo <= hi) {

            int mid = lo + (hi - lo) / 2;

            if(check(v, n, t, mid) == 1) {

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

  

        int _t = 1;

        // cin >> _t;

  

        while(_t--) solve();

        return 0;

    }
```