[[DSA]]
Source: [[Leetcode]]
Topics: [[Prefix Sum]], [[Array]]
Difficulty: [[Medium]]

Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]

## Approach
The most efficient way to do this question in O(n) time and O(1) space complexity is by used prefix and postfix sum.

Let's solve the first example - [1, 2, 3, 4]
We will first create a prefix array while keeping track of the prefix

1. Initialize prefix
	The prefix array will always have its first value as 1 as the value of all it's previous elements will be 1 (default).
	Array - [1]
2. 1
	Multiply the first value (1), with the prefix and store it in the next position
	Prefix = 1
	Array - [1, 1]
3. 2
	Multiply the next value (2), with the prefix and store it in the next position
	Prefix = 2
	Array - [1, 1, 2]
4. 3
	Multiply the next value (3), with the prefix and store it in the next position
	Prefix = 6
	Array - [1, 1, 2, 6] 

The last element will be skipped as per the problem statement.
Then we move on to the postfix calculation on the same array

1. Initialize postfix
	Similar postfix will also always start with 1.
	Postfix = 1
	Multiply postfix with the last element of the array and store it in the same position
	1 * 6 = 6
	Array - [1, 1, 2, 6]
	Then we multiply the postfix with the last element of the original array i. e. 4
	Postfix = 1 * 4 = 4
2. Next element
	Multiplying postfix with the next element in the array we get
	Array - [1, 1, 8, 6]
	Then we multiply postfix with the next element from the original array 
	Postfix = 4 * 3 = 12
3. Next element
	Multiplying postfix with the next element in the array we get
	Array - [1, 12, 8, 6]
	Then we multiply postfix with the next element from the original array 
	Postfix = 12 * 2 = 24
4. Next element
	Multiplying postfix with the next element in the array we get
	Array - [24, 12, 8, 6]
	Thus the answer has been found

But why are we doing this,
For each element in the array, we want to basically multiply all the elements to it's left and then multiply them with all the elements to the right. 
![[Problem- 238. Product of Array Except Self Explanation.png]]
## Code
```
class Solution {

public:

    vector<int> productExceptSelf(vector<int>& nums) {

        int n = nums.size();

        vector<int> ans(n);

        // calculate and store prefix values

        int pre = 1;

        for(int i = 0; i < n; i++) {

            ans[i] = pre;

            pre *= nums[i];

        }

  

        // calcualte postfix values and then multiply with prestored prefix values

        int post = 1;

        for(int i = n - 1; i >= 0; i--) {

            ans[i] *= post;

            post *= nums[i];

        }

  

        return ans;

    }

};
```
