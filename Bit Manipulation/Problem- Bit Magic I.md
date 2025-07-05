Source: [[AlgoZenith]]
Topics: [[Bit manipulation]]
Difficulty: [[Hard]]

Following is the description of this [[DSA]] problem. 

A state with **n** flags of ON or OFF can be represented by a sequence of bits where **0,1,...,n−1** -th flag corresponds to 1 (ON) or 0 (OFF). The state can be managed by the corresponding decimal integer because the sequence of bits is a binary representation where each bit is 0 or 1.

Given a sequence of bits with **60 flags** that represent a state, perform the following operations. Note that each flag of the bits is initialized by OFF.

1. test(i): Print 1 if **ith** flag is ON, otherwise 0.
2. set(i): Set **ith** flag to ON
3. clear(i): Set **ith** flag to OFF
4. flip(i): Inverse **ith** flag
5. all: Print 1 if all flags are ON, otherwise 0
6. any: Print 1 if at least one flag is ON, otherwise 0
7. none: Print 1 if all flags are OFF, otherwise 0
8. count: Print the number of ON flags
9. val: Print the decimal value of the state

## Approach
It's obvious we have to use [[Bit manipulation]] in this problem. 
All the methods are pretty easy but the way to get the count is interesting.
So when u subtract 1 from a number, all the bits after the 1st 1 from the right get flipped.

So for eg. 10 = 1010
will become
9 = 1001

And then when we perform a bitwise & on them, we get 1000. 
And we set that as the new number i.e 8
We subtract 1 again and get 7 = 0111
After & we get 0

At every step, we increment the count. So here, we would have incremented count twice, which is exactly the number of 1s in the initial number. That's so cool. 

### Code 
```
#include <bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

int test(int i, int &num) {

return (num >> i) & 1;

}

  

void set_flag(int i, int &num) {

num = num | (1LL << i);

}

  

void clear(int i, int &num) {

num = num & ~(1LL << i);

}

  

void flip(int i, int &num) {

num = num ^ (1LL << i);

}

  

int all(int &num) {

return __builtin_popcountll(num) == 60;

}

  

int any_flag(int &num) {

return num > 0;

}

  

int none(int &num) {

return num == 0;

}

  

int count_1s(int num) {

int count = 0;

while(num) {

count++;

num = num & (num - 1);

}

return count;

}

  

void solve() {

int q; cin >> q;

int num = 0;

while(q--) {

int func; cin >> func;

int i;

switch(func) {

case 1:

cin >> i;

cout << test(i, num) << endl;

break;

  

case 2:

cin >> i;

set_flag(i, num);

break;

  

case 3:

cin >> i;

clear(i, num);

break;

  

case 4:

cin >> i;

flip(i, num);

break;

  

case 5:

cout << all(num) << endl;

break;

  

case 6:

cout << any_flag(num) << endl;

break;

  

case 7:

cout << none(num) << endl;

break;

  

case 8:

cout << count_1s(num) << endl;

break;

  

case 9:

cout << num << endl;

break;

  

default:

break;

}

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
