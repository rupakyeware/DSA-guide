Source: [[CodeForces]]
Difficulty: [[900]]
Topics: [[Greedy]], [[Math]]

In this [[DSA]] problem, the cost of a positive integer 𝑛 is defined as the result of dividing the number 𝑛 by the sum of its digits.

For example, the cost of the number 104 is 1041+0+4=20.8, and the cost of the number 111 is 1111+1+1=37.

You are given a positive integer 𝑛 that does not contain leading zeros. You can remove any number of digits from the number 𝑛 (including none) so that the remaining number contains at least one digit and is strictly greater than zero. The remaining digits cannot be rearranged. As a result, you may end up with a number that has leading zeros.

For example, you are given the number 103554. If you decide to remove the digits 1, 4, and one digit 5, you will end up with the number 035, whose cost is 0350+3+5=4.375.

What is the minimum number of digits you need to remove from the number so that its cost becomes the minimum possible?

Input

The first line contains an integer 𝑡 (1≤𝑡≤1000) — the number of test cases.

The only line of each test case contains a positive integer 𝑛 (1≤𝑛<10100) without leading zeros.

Output

For each test case, output one integer on a new line — the number of digits that need to be removed from the number so that its cost becomes minimal.

Example

input

```
4

666
13700
102030
7
```

output

```
2
4
3
0
```

## Approach- 
The approach I was thinking of was to find the largest number in the sequence, remove all the digits after it, and remove all the non zero digits before it.
This resulted in a wrong answer. That's because, we do not have to find the largest digit. We just have to bring the cost down to 1. That can be done with any number. 

But we have to find such a number that will take the minimum amount of digit removal.
Another observation is that 0s on the left are good- since they have no value.
But 0s on the right are just as expensive as any other number.

So, we find the rightmost non-zero digit, remove all the 0s after it and then iterate from 0 up to the index of the rightmost digit while removing any non zero elements.

By 'removing' here I don't mean removing. I just mean increasing the count of the numbers removed.

### Code 
```
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

string num; cin >> num;

int right_most_digit = -1, idx;

int len = num.size();

  

// Find the right most non zero digit

for(int i = 0; i < len; i++) {

int val = num[i] - '0';

if(val) {

right_most_digit = val;

idx = i;

}

}

// Remove all digits after idx

int numsRemoved = 0;

numsRemoved += len - idx - 1;

  

// Remove all non-zero digits before idx

for(int i = 0; i < idx; i++) {

if(num[i]-'0' > 0) {

numsRemoved++;

}

}

  

cout << numsRemoved << endl;

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