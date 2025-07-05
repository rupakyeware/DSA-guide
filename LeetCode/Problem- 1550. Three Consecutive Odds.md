Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Array]]

In this [[DSA]] problem, given an integer array `arr`, return `true` if there are three consecutive odd numbers in the array. Otherwise, return `false`.

**Example 1:**

**Input:** arr = [2,6,4,1]
**Output:** false
**Explanation:** There are no three consecutive odds.

**Example 2:**

**Input:** arr = [1,2,34,3,4,5,7,23,12]
**Output:** true
**Explanation:** [5,7,23] are three consecutive odds.

## Solution

``` cpp
class Solution {

public:

bool threeConsecutiveOdds(vector<int>& arr) {

int countOfOdds = 0;

int n = arr.size();

  

for(int i = 0; i < n; i++) {

countOfOdds = (arr[i] % 2 ? countOfOdds + 1 : 0);

if(countOfOdds == 3) return true;

}

  

return false;

}

};
```