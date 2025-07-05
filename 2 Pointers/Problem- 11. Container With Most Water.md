Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]]

In this [[DSA]] problem, you are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return _the maximum amount of water a container can store_.

**Notice** that you may not slant the container.

![[Problem 11. Container With Most Water Image.png]]

## Approach
Imagine this. You place [[2 pointers]], one at the beginning of the array and one at the end. At any given point in time, you can only see the bars that are pointed to by the pointers. So we compute the area formed by these points by multiplying the minimum between the two bars (as the max water than can be filled in a container formed by the 2 bars is up to the height of the smaller bar) with the distance between the 2 bars.

Then comes the key part. We move the smaller bar inward. Why? We have no clue what the bars are present other than the ones we have already ones. So there MIGHT be a bigger bar. But we want to compute the best answer. Which means getting rid of the smaller bar of the 2 to potentially replace it with a bigger bar and maybe get a better answer.

```
class Solution {

public:

int maxArea(vector<int>& height) {

int n = height.size();

int l = 0, r = n - 1; // Set the 1st pointer to the start and second to the end

int maxArea = 0;

while(l < r) { // while we can still traverse a pair of bars

int currArea = min(height[l], height[r]) * (r - l); // area = height of smaller bar * dist between the bars

maxArea = max(currArea, maxArea);

  

// try to replace the smaller bar with a bigger bar

if(height[l] < height[r]) l++;

else r--;

}

  

return maxArea;

}

};
```
