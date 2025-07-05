Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Array]], [[2 pointers]]

In this [[DSA]] problem, you are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
**Output:** [1,2,2,3,5,6]
**Explanation:** The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example 2:**

**Input:** nums1 = [1], m = 1, nums2 = [], n = 0
**Output:** [1]
**Explanation:** The arrays we are merging are [1] and [].
The result of the merge is [1].

**Example 3:**

**Input:** nums1 = [0], m = 0, nums2 = [1], n = 1
**Output:** [1]
**Explanation:** The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

**Constraints:**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

**Follow up:** Can you come up with an algorithm that runs in `O(m + n)` time?

## Approach 1- Using sort()
This is the most naive approach which will take O(m + nlogn) time. We just copy the m elements from num2 into the last m elements kept as 0 in nums1 and then run the sort function over it. 

### Code 
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for(int i = m, j = 0; i < m+n; i++, j++) {
            nums1[i] = nums2[j];
        }

        sort(nums1.begin(), nums1.end());
    }
};
```


## Approach 2- Using 2 pointers 
This approach solves the problem in O(m+n) time. We will technically need 3 pointers but the 3rd pointer only does the job of pointing at the place of insertion in the nums1 vector. 
So we set i = m-1, j = n-1 and k = m+n-1. Then if nums1[i] > nums2[j], we set the kth element in nums1 equal to nums1[i] and vice versa if nums2[j] was greater. We keep doing this until j >= 0 since even if some elements from nums1 are left to be traversed by i, they will already be in the right position.

### Code 
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1;
        int j = n-1;
        int k = m+n-1;

        while(j >= 0) {
            if(i >= 0 && nums1[i] >= nums2[j]) {
                nums1[k] = nums1[i];
                i--;
            }
            else {
                nums1[k] = nums2[j];
                j--;
            }
            k--;
        }
    }
};
```