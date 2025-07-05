Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Binary Search]], [[2 pointers]]

In this [[DSA]] problem, given a **1-indexed** array of integers `numbers` that is already **_sorted in non-decreasing order_**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return _the indices of the two numbers,_ `index1` _and_ `index2`_, **added by one** as an integer array_ `[index1, index2]` _of length 2._

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

**Example 1:**

**Input:** numbers = [2,7,11,15], target = 9
**Output:** [1,2]
**Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

**Example 2:**

**Input:** numbers = [2,3,4], target = 6
**Output:** [1,3]
**Explanation:** The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].

**Example 3:**

**Input:** numbers = [-1,0], target = -1
**Output:** [1,2]
**Explanation:** The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].

## Approach 1 - Binary Search (The one I thought of)
I thought of using a simple binary search on every start. The equation is x + y = target. So I need to iterate through the Xs (i) in the array and check if target - y exists using binary search. 

For the binary search, lo will be i + 1, hi will be n - 1. And we may not get an answer in every iteration, but an answer is guaranteed to be found. So you can initialize it to anything. It doesn't matter.

### Code
```
class Solution {

public:

    vector<int> twoSum(vector<int>& numbers, int target) {

        int n = numbers.size();

  

        for(int i = 0; i < n - 1; i++) {

            int lo = i + 1, hi = n - 1, ans = -1;

            while(lo <= hi) {

                int mid = lo + (hi - lo) / 2;

                int sum = numbers[i] + numbers[mid];

                if(sum == target) {

                    return {i + 1, mid + 1};

                }

                if(sum < target) lo = mid + 1;

                else hi = mid - 1;

            }

        }

  

        return {};

    }

};
```

## Approach 2 - 2 pointers (NeetCode 150)
This approach is much simpler and easier to understand. It also solves the problem in O(n).
Basically, we set the left pointer(l) to 0 and the right pointer(r) to n - 1. Then we check if l + r == target. If yes return l+1 and r +1 (+1 because the ans has to be 1-indexed) in vector form. But if the sum is lesser than target, we move the left pointer 1 element ahead. Else if it's greater, we move the right pointer 1 element behind.  

This is genius. The array is already sorted in the question. Even if it wasn't we can just sort it and the answer would be the same. This is an amazing approach.

### Code 
```
class Solution {

public:

    vector<int> twoSum(vector<int>& numbers, int target) {

        int n = numbers.size();

        int l = 0, r = n - 1;

  

        while(numbers[l] + numbers[r] != target) {

            int sum = numbers[l] + numbers[r];

            if(sum < target) l++;

            else r--;

        }

  

        return {l+1, r+1};

    }

};
```
