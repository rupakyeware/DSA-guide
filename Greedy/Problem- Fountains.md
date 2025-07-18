Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[Greedy]]

In this [[DSA]] problem, you are given a one-dimensional garden of length `n`, where there is a fountain at every position from `1` to `n`.

Each fountain `i` (1-based index) has a coverage range described by the array `locations[]`. A fountain at position `i` can water positions from:

`max(1, i - locations[i]) to min(n, i + locations[i])`

In other words, the fountain's spray radius can extend `locations[i]` units to the left and right, without going beyond the garden bounds.

Initially, all fountains are turned **off**. Your task is to determine the **minimum number of fountains** that need to be **activated** so that the **entire garden** from position `1` to `n` is fully covered.

---

## 🧾 Input Format

- The first line contains an integer `T` — the number of test cases.
    
- For each test case:
    
    - The first line contains a single integer `n` — the length of the garden.
        
    - The second line contains `n` space-separated integers — the `locations[]` array.
        

---

## 📤 Output Format

- For each test case, output a single integer — the minimum number of fountains to activate to cover the entire garden.
    

---

## 🔒 Constraints

- 1 ≤ T ≤ 10⁵
    
- 1 ≤ n ≤ 10⁵
    
- 0 ≤ locations[i] ≤ min(n, 100)
    
- Sum of all `n` across all test cases ≤ 10⁵
    

---

## 🧪 Sample Input 1


`1 3 1 1 1`

### Sample Output 1


`1`

---

## 🧪 Sample Input 2


`1 3 1 0 1`

### Sample Output 2

`2`

---

## 🧪 Sample Input 3

`1 3 0 0 0`

### Sample Output 3

`3`

---

## 📝 Explanation

- **Test Case 1**: Activating only the **middle fountain** (index 2) will cover positions 1 to 3.
    
- **Test Case 2**: Middle fountain covers nothing. Need to activate fountain 1 (covers 1–2) and fountain 3 (covers 2–3).
    
- **Test Case 3**: Every fountain covers only itself. All 3 need to be activated to cover 1 to 3.

## Approach 
We will use a [[Greedy]] approach. For that, first we'll get the extents of each fountain and store that at the idx of the left extent. This essentially means that at the current point, some fountain which was turned on ahead of where we currently are, could reach a distance of x. 
Then we will make an array that will store the max going from left to right to ensure we always store the optimal value and don't overwrite it with a lesser one.

Finally, in the while loop, curr is where we are, and count is the number of fountains considered. From the current position, we will go to the position stored in the maxReach array for the current position. So basically, we are turning on a fountain, and taking a hop to where the fountain ends, and repeat.

### Code 
``` cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> locations(n);
    for (int i = 0; i < n; i++) cin >> locations[i];

    vector<int> ranges(n);
    for (int i = 0; i < n; i++) {
        int left = max(i - locations[i], 0);
        int right = min(i + locations[i], n - 1);
        ranges[left] = max(ranges[left], right);
    }

    vector<int> maxReach(n);
    maxReach[0] = ranges[0];
    for (int i = 1; i < n; i++)
        maxReach[i] = max(maxReach[i - 1], ranges[i]);

    int curr = 0, count = 0;
    while (curr < n) {
        count++;
        int nextCurr = maxReach[curr];
        if (nextCurr < curr) break;
        curr = nextCurr + 1;
    }

    cout << count << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T;
    while (T--) {
        solve();
    }
    return 0;
}
```
