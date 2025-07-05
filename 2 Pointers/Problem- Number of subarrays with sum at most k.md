Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[2 pointers]]

In this [[DSA]] problem, given an array of _N_ integers, find the number of subarrays with a sum less than equal to _K_.

## Approach 
I thought of a solution but I didn't think of a condition and that led to a wrong answer. Then I asked Claude to help me and I got this solution which is much easier to understand and even code.

The approach was pretty simple. 2 pointers, one tail and one head, each initialized to start from the 0th index.

We will keep iterating till the head reaches n
We want to consume the head'th element and then check if it's a valid subarray. If it isn't, we will move the tail'th element ahead to discard it and hopefully get a subarray with sum at most k. If not, we discard more tail elements until we do.

Every time we reach a valid subarray, we append head - tail + 1 as that's the number of valid subarrays in the current window.

And then we increment head to consume the next element.

```
#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

int n, k; cin >> n >> k;

vector<int> vec(n);

for(int i = 0; i < n; i++) cin >> vec[i];

  

int tail = 0, head = 0;

int currSum = 0;

int cnt = 0;

while(head < n) {

currSum += vec[head];

  

while(currSum > k && tail <= head) {

currSum -= vec[tail];

tail++;

}

  

cnt += head - tail + 1;

  

head++;

}

  

cout << cnt << endl;

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
