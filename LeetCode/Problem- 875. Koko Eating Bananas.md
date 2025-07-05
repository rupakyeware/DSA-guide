Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]] on [[Answer Space]]

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return _the minimum element of this array_.

You must write an algorithm that runs in `O(log n) time`.

**Example 1:**

**Input:** nums = [3,4,5,1,2]
**Output:** 1
**Explanation:** The original array was [1,2,3,4,5] rotated 3 times.

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2]
**Output:** 0
**Explanation:** The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

**Example 3:**

**Input:** nums = [11,13,15,17]
**Output:** 11
**Explanation:** The original array was [11,13,15,17] and it was rotated 4 times.

## Approach 
 A question similar to this was done in [[AlgoZenith]] so I was able to do this one pretty easily. 
 We want to perform a binary search on the answer space space here. What this means is that the 'mid' we will be calculating at each iteration won't be an element, but the value of the bananas per hour. If all the bananas can be eaten in 'mid' bph (bananas per hour), we try to find a lower value. 

### Code 
```cpp
class Solution {

public:

bool check(vector<int> &piles, int bph, int h) {

long long int hours_taken = 0;

int n = piles.size();

for(int i = 0; i < n; i++) {

if(piles[i] <= bph) hours_taken++;

else {

if(piles[i] % bph) hours_taken += (piles[i] / bph) + 1;

else hours_taken += (piles[i] / bph);

}

}

  

return hours_taken <= h;

}

int minEatingSpeed(vector<int>& piles, int h) {

sort(piles.begin(), piles.end());

// find value of hi (total bananas in pile)

int maxVal = 0;

for(int i: piles) maxVal = max(maxVal, i);

  

// binary search on the answer space

int lo = 1, hi = maxVal, ans = maxVal;

while(lo <= hi) {

int mid = lo + (hi - lo) / 2;

if(check(piles, mid, h)) {

ans = mid;

hi = mid - 1;

}

else {

lo = mid + 1;

}

}

  

return ans;

}

};
```

