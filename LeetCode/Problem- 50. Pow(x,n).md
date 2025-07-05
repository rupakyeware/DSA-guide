Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Math]], [[Recursion]]

In this [[DSA]] problem, Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

**Example 1:**

**Input:** x = 2.00000, n = 10
**Output:** 1024.00000

**Example 2:**

**Input:** x = 2.10000, n = 3
**Output:** 9.26100

**Example 3:**

**Input:** x = 2.00000, n = -2
**Output:** 0.25000
**Explanation:** 2-2 = 1/22 = 1/4 = 0.25

**Constraints:**

- `-100.0 < x < 100.0`
- `-231 <= n <= 231-1`
- `n` is an integer.
- Either `x` is not zero or `n > 0`.
- `-104 <= xn <= 104`

## Approach 
Refer to the explanation in the below image
![[Screenshot 2025-07-04 at 9.47.35 PM.png]]

### Code 
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if(n == 0) return 1;
        long long N;
        if(n < 0) {
            N = -1 * (long long) n;
        }
        else {
            N = n;
        }
        double ans = myPow(x, N/2);
        ans *= ans;
        if(N%2) ans *= x;
        return (n > 0 ? ans : 1/ans);
    }
};
```