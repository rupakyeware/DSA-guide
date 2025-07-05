Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Multiset]], [[Monotonic Deque]], [[Deque]], [[Array]]
In this [[DSA]] problem, we are given an array of size n, and an integer k.
We have to find the minimum value of each subarray of size k.

---
### Approach 1. Brute force
We can iterate through the array from 0-2, find the minimum. Then iterate from 1-3, find the minimum and so on. But that will take n^2 time complexity. So how can we optimise this.

---

In [[How optimization happens in DSA]] we saw that one of the ways of optimization of DSA problems is with the sharing of information. So that's exactly what we'll do.
We need to store the minimum value at a given point of the array while making sure we don't take the old subarrays into consideration.

---
### Approach 2. [[Multiset]]
We will iterate through the array and store the ith value in a multiset while making sure that if i >= k, then the (i-k)th is deleted. And if i >= k, then we'll also print the value at the beginning of the multiset. This approach will take O(nlog(n)) time.

#### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

    int n, k; cin >> n >> k;

    vector<int> v(n);

    for(int i = 0; i < n; i++) cin >> v[i];

  

    multiset<int> ms;

    // Why multiset?

    // Can store multiple occurences in ordered fashion.

    // Insert in log(n). Erase in log(n). Easily retrieve min/max of elements.

  

    //Insert the indexes of the elements in v into ms

    //If i >= k - 1, print min

    // If i > k, erase v[i-k] from ms

  

    for(int i = 0; i < n; i++) {

        ms.insert(v[i]);

        if(i >= k) ms.erase(v[i-k]);

        if(i >= k - 1) cout << *ms.begin() << " ";

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


### Approach 3. [[Monotonic Deque]]
To do the above in O(n) time, we use a monotonic deque. The approach is similar to multiset when it comes to removing an element. But when inserting, we keep popping elements from the back until the element to be inserted is >= the element at the back.

#### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

class montone_deque {

    deque<int> dq;

    public:

    void insert(int x) {

        while(!dq.empty() && dq.back() > x) dq.pop_back();

        dq.push_back(x);

    }

  

    void erase(int x) {

        if(dq.front() == x) dq.pop_front();

    }

  

    int getMin() {

        return dq.front();

    }

};

  

void solve() {

    int n, k; cin >> n >> k;

    vector<int> v(n);

    for(int i = 0; i < n; i++) cin >> v[i];

  

    montone_deque md;

  

    for(int i = 0; i < n; i++) {

        md.insert(v[i]);

        if(i >= k) md.erase(v[i-k]);

        if(i >= k - 1) cout << md.getMin() << " ";

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

