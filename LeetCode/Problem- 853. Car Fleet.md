Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]]

In this [[DSA]] problem, there are `n` cars at given miles away from the starting mile 0, traveling to reach the mile `target`.

You are given two integer array `position` and `speed`, both of length `n`, where `position[i]` is the starting mile of the `ith` car and `speed[i]` is the speed of the `ith` car in miles per hour.

A car cannot pass another car, but it can catch up and then travel next to it at the speed of the slower car.

A **car fleet** is a car or cars driving next to each other. The speed of the car fleet is the **minimum**speed of any car in the fleet.

If a car catches up to a car fleet at the mile `target`, it will still be considered as part of the car fleet.

Return the number of car fleets that will arrive at the destination.

**Example 1:**

**Input:** target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]

**Output:** 3

**Explanation:**

- The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12. The fleet forms at `target`.
- The car starting at 0 (speed 1) does not catch up to any other car, so it is a fleet by itself.
- The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches `target`.

**Example 2:**

**Input:** target = 10, position = [3], speed = [3]

**Output:** 1

**Explanation:**

There is only one car, hence there is only one fleet.

**Example 3:**

**Input:** target = 100, position = [0,2,4], speed = [4,2,1]

**Output:** 1

**Explanation:**

- The cars starting at 0 (speed 4) and 2 (speed 2) become a fleet, meeting each other at 4. The car starting at 4 (speed 1) travels to 5.
- Then, the fleet at 4 (speed 2) and the car at position 5 (speed 1) become one fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches `target`.

**Constraints:**

- `n == position.length == speed.length`
- `1 <= n <= 105`
- `0 < target <= 106`
- `0 <= position[i] < target`
- All the values of `position` are **unique**.
- `0 < speed[i] <= 106`

## Approach - 
This was a pretty fun problem to solve. I tried to come up with the solution myself but couldn't, even after looking at the hints. So I had to watch the solution video.

First we want to make a vector of pairs where the pair will be `position[i], speed[i]`
Then we will sort it in decreasing order since it's easier to find intersections if we go backwards. 
After that, we will make a stack of pairs call fleets. 

Then we will iterate through the sorted vector and for every element we will calculate the time it will need to reach the target from its starting position using `(target - position[i])/speed[i]`. The position and speed will already be stored in the pair making them easier to access. If the stack is empty of the current time is greater than the time at the top of the stack, we push the current element to the stack. 

This is because since the current car is starting behind the car at the top of the stack AND it's taking more time, it will never form a fleet with the car at the top. And we want the number of cars in the stack in the end to be the number of fleets. If a car is taking less time to reach the target in lesser time than the top car even though it's starting behind, it's definitely going to intersect with the top car, hence we don't have to add it to the stack.

At the end, after iterating through the entire array, we return the size of the stack.

### Code 
```
class Solution {

public:

int carFleet(int target, vector<int>& position, vector<int>& speed) {

int len = position.size();

stack<float> fleets;

vector<pair<int, int>> cars(len);

for(int i = 0; i < len; i++) cars[i] = {position[i], speed[i]};

sort(cars.begin(), cars.end(), greater<pair<int, int>>()); // Sort pairs in decreasing order

  

for(int i = 0; i < len; i++) {

float time = float((target - cars[i].first)) / float(cars[i].second); // Calculate time required for current car to reach target

if(fleets.empty() || fleets.top() < time) fleets.push(time); // If current car is slower than the car at the top of the stack

}

return fleets.size();

}

};
```