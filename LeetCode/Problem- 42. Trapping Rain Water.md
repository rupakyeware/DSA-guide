Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Array]], [[2 pointers]]

Another problem that was very easy to solve when I understood the approach from neetcode.

In this [[DSA]] problem, given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.
**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
**Output:** 6
**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example 2:**

**Input:** height = [4,2,0,3,2,5]
**Output:** 9

## Approach 1- Using 2 arrays
This approach takes O(n) time as well as O(n) memory

In order to find out how much water can be trapped at any given point, we need to know what is the max height to its left and the max height to its right. The way we do that is by just iterating through the height array twice, once from left to right to find the max_left height and once from right to left to find the max_right height.

Then to calculate the amount of water that can be stored, we use the formula
`min(height[r], height[l]) - height[i]`
Because the water that can be stored at max at any given point, will be equal to the closest height to it - current height of the point

If the value obtained for a point is greater than 0, it means water can be stored there. So we add that to the tracking variable.

### Code 

```
class Solution {

public:

int trap(vector<int>& height) {

int n = height.size();

vector<int> max_l(n), max_r(n);

  

// Calculate the max value to left of the ith height

int highest = 0;

for(int i = 0; i < n; i++) {

max_l[i] = highest;

highest = max(highest, height[i]);

}

  

// Calculate the max value to the right of the ith height

highest = 0;

for(int i = n - 1; i >= 0; i--) {

max_r[i] = highest;

highest = max(highest, height[i]);

}

  

int water_trapped = 0;

for(int i = 0; i < n; i++) {

int trapped = min(max_l[i], max_r[i]) - height[i];

if(trapped > 0) water_trapped += trapped;

}

  

return water_trapped;

}

};
```

## Approach 2- [[2 pointers]]
This approach uses O(n) time and O(1) space.

This approach is very interesting. We don't compare the max left and max right to find the minimum height and then subtract the current height from it. 
We keep track of two pointers, l and r. Along with that, we also keep track of max_l and max_r; 
We compare the height of l and r and move the smaller one inside by one step and update the max_l or max_r accordingly. Then we calculate the water that can be stored at the point that was just moved.

For eg. if the left pointer was moved, then the water that can be stored at point l is going to be `max_l - height[l]`. Same with the right pointer. 

The approach I used to update the total water tracked is to keep a last_added variable and update the water calculation in that. If the value is > 0, then add it to the total water tracked and reset it to 0. If this was hard to understand, reading the code will help make it easier.

## Code 
```
class Solution {

public:

int trap(vector<int>& height) {

int n = height.size();

int water_trapped = 0, last_trapped = 0;

int l = 0, r = n-1;

int max_left = 0, max_right = 0;

while(l < r) {

if(last_trapped > 0) {

water_trapped += last_trapped;

last_trapped = 0;

}

if(height[l] <= height[r]) {

max_left = max(max_left, height[l]);

l++;

last_trapped = max_left - height[l];

}

else {

max_right = max(max_right, height[r]);

r--;

last_trapped = max_right - height[r];

}

}

  

return water_trapped;

}

};
```
