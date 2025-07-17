Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Greedy]], [[Dynamic Programming]], [[Recursion]]

In this [[DSA]] problem, you are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return _the minimum number of jumps to reach_ `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** 2
**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [2,3,0,1,4]
**Output:** 2

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.

## Approach 
Like [[Problem- 55. Jump Game]] this problem can be solved with dp too but I'll focus more on the optimal greedy approach with O(n) space and O(1) time.

This is a very classical pattern of problems where we have to find the farthest extend of the position we are currently standing at and it usually has some sort of counting involved. Like here, we have to count the minimum number of steps needed to reach the end.

So we will start iterating on the array while keeping a track of the end of the current interval, farthest we can go right now, and number of jumps taken.

For every iteration, we'll first update farthest with the max of farthest and i + nums[i] (farthest jump we can take right now). Then we'll check if we're at the end of our current interval with if(i == currEnd). If we are, we have to increment the count of jumps and then set currEnd = farthest.

We'll keep doing this for n-1 steps and return the number of jumps taken.

### Code 
```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int currEnd = 0, farthest = 0, jumps = 0;

        for(int i = 0; i < nums.size()-1; i++) {
            farthest = max(farthest, i+nums[i]);

            if(currEnd == i) {
                jumps++;
                currEnd = farthest;
            }
        }

        return jumps;
    }
};
```