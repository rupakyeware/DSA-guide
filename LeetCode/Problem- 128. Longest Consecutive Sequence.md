Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Unordered Map]], [[Unordered Set]]

In this [[DSA]] problem, you are given an unsorted array of integers `nums`. You have to return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

```
Example 1:

Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

Example 2:
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

Example 3:

Input: nums = [1,0,1,2]
Output: 3
```
**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

### Approach
Solving this in n^2 or even nlog(n) is easy. The tricky part is doing it in n. But now that I understood the approach, it doesn't seem that tricky anymore.

Basically, just make an [[Unordered Set]] of all the elements in the nums array. You can also do it with an [[Unordered Map]] of Boolean with a similar approach. 

Then our approach is to iterate over all the elements in the nums array and check if the current element can be the start of a sequence. If it can, then calculate the length of the sequence it can form and update maxLen if needed. 

The way we do it in set is by finding if nums[i-1] does not exist in the set. 

I made the mistake of iterating over the nums array to check the start of sequences. But the nums array can also contain duplicates, and we don't care about duplicates in the sequence, just the values. That's why, ==iterating over the set itself, is a better idea== else we will get a TLE.

``` cpp
class Solution {

public:

    int longestConsecutive(vector<int>& nums) {

        int n = nums.size();

        if(!n) return 0;

  

        unordered_set<int> uset;

        for(int num: nums) uset.insert(num);

  

        int maxLen = 1;

        for(int num: uset) {

            if(uset.find(num - 1) == uset.end()) {

                int currNum = num;

                int currLen = 1;

  

                while(uset.find(currNum + 1) != uset.end()) {

                    currLen++;

                    currNum++;

                }

  

                maxLen = max(maxLen, currLen);

            }

        }

  

        return maxLen;

    }

};
```

