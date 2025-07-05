Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Array]]

In this [[DSA]] problem, given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]
**Output:** `[[-1,-1,2],[-1,0,1]]`
**Explanation:** 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.

**Example 2:**

**Input:** nums = [0,1,1]
**Output:** []
**Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

**Input:** nums = [0,0,0]
**Output:** [0,0,0]
**Explanation:** The only possible triplet sums up to 0.

### Approach 
Let's say the 3 values that make the triplet are v1, v2, v3. We will iterate from 0 to n - 2 to find all possible values of v1. If the current value of v1 is the same as the last one we encountered, skip it.

Then, we approach the problem similar to [[Problem- 167. Two Sum II - Input Array Is Sorted]] where set l and r to the edges of the boundary. Then check if the sum formed by the 3 values is 0. If it is, add it to the answer vector. Then we set the next l to the next unique value and the same is done with r. Else, if the value is > 0, we move the right pointer one step back. Else, if the value is < 0, we move the left pointer one step ahead.

This was pretty easy. Still needed help from NeetCode 150 though to recognise the algorithm. 

```
class Solution {

public:

    vector<vector<int>> threeSum(vector<int>& nums) {

        vector<vector<int>> ans;

  

        sort(nums.begin(), nums.end()); // Sort the input vector so we can make decisions on which pointer to move

        int n = nums.size();

  

        for(int i = 0; i < n - 2; i++) { // we iterate through all possible combinations of the first pointer

            if(i && nums[i] == nums[i-1]) continue; // if current value is the same as last value encountered

            int l = i + 1, r = n - 1; // set boundaries of l and r

            while(l < r) {

                int sum = nums[i] + nums[l] + nums[r]; // compute the current sum being formed by the 3 values

                if(sum > 0) r--; // sum is bigger than 0 so check for a smaller 3rd value

                else if(sum < 0) l++; // sum is lesser than 0 so check for a bigger 2nd valuee

                else { // We received a correct triplet

                    ans.push_back({nums[i], nums[l], nums[r]}); // Add triplet to ans vector

                    while(l < r && nums[l] == nums[l+1]) l++; // While element next to l is the same as l, skip

                    l++; // move l one step ahead

                    r--; // move r one step behind

                }

            }

        }

  

        return ans;

    }

};
```
