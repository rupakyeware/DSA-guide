Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[Bit manipulation]]

In this [[DSA]] problem, given an integer **t** - the number of test cases. For each test case, read an integer **n** and answer the following 6 queries in the given order in a new line -:

1. Output the 64 bit binary representation of number **n**.
2. Output the **position** of the most significant bit ( **MSB** ) or the leftmost set bit of **n** ( 0- indexed ). If n=0, output -1.
3. Output the **position** of the rightmost set bit of **n** ( 0- indexed ). If n=0, output -1.
4. Output 1 if **n**  is a power of 2 i.e n=2X , x > 0, else output 0.
5. Output the biggest power of 2, that is the divisor of **n.** i.e biggest **2K** such that **n%2K == 0, k ≥ 0.** If n=0, output -1. 
6. Output the smallest power of 2, which is not smaller than **n,** i.e smallest **2K** such that **2K ≥ n, k>0.**

##### Input Format

First-line contains an integer **t -**  number of test cases.

Each next **t** lines contain an integer **n.**

### Code 
```
#include "../stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

string give64Bit(int n) {

string ans;

for(int i = 63; i >= 0; i--) {

ans.push_back((1 & (n >> i)) + '0');

}

return ans;

}

  

int msbPosition(int n) {

for(int i = 63; i >= 0; i--) {

int bit = 1 & (n >> i);

if(bit) {

return i;

}

}

return -1;

}

  

int lsbPosition(int n) {

for(int i = 0; i < 64; i++) {

if(1 & (n >> i)) return i;

}

return -1;

}

  

bool isPowerOf2(int n) {

for(int i = 1; i < 63; i++) {

if((1LL << i) == n) return true;

}

return false;

}

  

int biggestPowerOf2IsDivisor(int n) {

if(n == 0) return -1;

int ans = 1;

for(int i = 1; i < 63; i++) {

int val = (1LL << i);

if(n % val == 0) ans = val;

}

return ans;

}

  

int smallestPowerOf2(int n) {

if(isPowerOf2(n)) return n;

for(int i = 1; i < 63; i++) {

int val = (1LL << i);

if(val > n) return val;

}

return 0;

}

  

void solve() {

int n; cin >> n;

  

// 64-bit representation

cout << give64Bit(n) << endl;

  

// position of MSB (0-indexed)

cout << msbPosition(n) << endl;

  

// position of LSB (0-indexed)

cout << lsbPosition(n) << endl;

  

// Is n a power of 2

cout << isPowerOf2(n) << endl;

  

// Biggest power of 2 that is a divisor of n

cout << biggestPowerOf2IsDivisor(n) << endl;

  

// Smallest power of 2 greater than n

cout << smallestPowerOf2(n) << endl;

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