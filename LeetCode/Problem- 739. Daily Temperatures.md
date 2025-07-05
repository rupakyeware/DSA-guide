Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]]

In this [[DSA]] problem, given an array of integers `temperatures` represents the daily temperatures, return _an array_`answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ `ith` _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0`instead.

**Example 1:**

**Input:** temperatures = [73,74,75,71,69,72,76,73]
**Output:** [1,1,4,2,1,1,0,0]

**Example 2:**

**Input:** temperatures = [30,40,50,60]
**Output:** [1,1,1,0]

**Example 3:**

**Input:** temperatures = [30,60,90]
**Output:** [1,1,0]

**Constraints:**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

## Approach 
The approach is very simple and interesting. We have to use a somewhat monotonic stack. The reason I used 'somewhat' is because the stack may contain duplicate elements so it won't always be decreasing. 

The stack will contain a pair of `temperature[i], idx[i]`. We want to iterate through the temperatures array. If the element at the top of the stack has a temperature less than the temperature[i] (current element) then we pop it from the stack and set it's idx in the ans vector to `i - st.top().second`, which is the distance between the two elements. 
Then we add the pair of the current element to the stack.
These steps are repeated until we finish iterating through the entire temperatures array.
### Code 
```
class Solution {

public:

vector<int> dailyTemperatures(vector<int>& temperatures) {

stack<pair<int,int>> st; // Kinda monotic stack to store {temperature, idx}

int len = temperatures.size();

vector<int> ans(len, 0); // Initializing with 0 reduces step of having to make indexes of elements with no greater value as 0

for(int i = 0; i < len; i++) {

// While an element with a lower temp exists at the top of the array

while(!st.empty() && temperatures[i] > st.top().first) {

pair<int, int> topItem = st.top();

ans[topItem.second] = i - topItem.second; // Set idx of lower temp = distance between it and currElem

st.pop(); // Pop the element

}

st.push({temperatures[i], i}); // Add current element to the stack

}

  

return ans;

}

};
```

