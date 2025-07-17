Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Array]]

In this [[DSA]] problem, a **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

## Approach 1- Brute force
A brute force approach would be to generate all the possible permutations of the given array and then return then run a linear search to find the current permutation and return exactly the next one after that. But that would take O(n * n!) time and the problem tells us to use only constant memory too so this won't work for this problem. 

## Approach 2- Optimal 
This is the approach I learned from Striver. It goes as follows 
1. Starting from the n-2th element, find the first decreasing element in the array (where arr[i] < arr[i+1])
2. If we found such an element, go to step 3. Else go to step 5
3. Find the first element greater than nums[i] starting from the end of the array 
4. Once we find that element, swap it with the ith element.
5. Reverse the array starting after the ith index. 
It's somewhat intuitive but not intuitive enough for me to be able to come up with myself in the an interview.
### Code 
