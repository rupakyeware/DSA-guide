Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]]

In this [[DSA]] problem, given an array of _N_ integers, find the number of subarrays with at most _K_ distinct elements.

Sample Input
```
5
3 2
1 2 3
3 2
3 2 2
5 0
2 1 0 4 0
7 3
1 2 1 0 1 0 2
10 5
1 0 7 1 10 2 4 10 1 3
```

Sample Output
```
5
6
0
28
46
```

## Approach 
We have to use 2 pointers, head and tail. Like any other 2 pointer problem, the head will consume the elements, while the tail will shit them out.

We have to keep a track of the number of distinct elements in the current window under consideration. We will do that by maintaining a frq array and incrementing the count of currCount when an element's frq count goes from 0 to 1.

If we reach a point where the currCount > k, we move the tail ahead while decrementing the count until the window is valid again.

Then we add the distance between the head and tail to the ans. After which, we move the head ahead.

```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

const int MAX = 1e5;

  

void solve() {

int n, k; cin >> n >> k;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

  

// Create a frequency array

vector<int> freq(MAX);

  

int tail = 0; // To delete element from window

int head = 0; // To consume element into window

int currCount = 0; // Num of distinct elements in current window

int ans = 0;

  

while(head < n) {

freq[vec[head]]++;

if(freq[vec[head]] == 1) currCount++;

  

while(currCount > k && tail <= head) {

if(freq[vec[tail]] == 1) currCount--;

freq[vec[tail]]--;

tail++;

}

  

ans += head - tail + 1;

head++;

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