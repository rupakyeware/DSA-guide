Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[priority queue]]

In this [[DSA]] problem, given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

**Input:** points = `[[1,3],[-2,2]],` k = 1
**Output:**` [[-2,2]]`
**Explanation:**
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just `[[-2,2]].`

**Example 2:**

**Input:** points = `[[3,3],[5,-1],[-2,4]],` k = 2
**Output:** `[[3,3],[-2,4]]`
**Explanation:** The answer `[[-2,4],[3,3]] `would also be accepted.

**Constraints:**

- `1 <= k <= points.length <= 104`
- `-104 <= xi, yi <= 104`

## Approach 1- Min heap 
This is the approach that I came up with which is miles slower than the optimal approach. The reason being, in my approach, insertion taken O(nlog(n)) whereas in the optimal approach, it takes O(nlog(k)). Big difference.

What I thought of was very straightforward. Add elements to a min heap according to distance and store the index of the point with that distance in a pair. 

Then for adding to the answer vector, just pop from the heap k times and add the elements at the position of the indexes we stored.

### Code 
``` cpp
class Solution {

public:

vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

priority_queue <pair<double, int>, vector<pair<double, int>>, greater<pair<double, int>>> pq;

vector<vector<int>> ans;

  

// Add the points to the min heap

int n = points.size();

for(int i = 0; i < n; i++) {

int x_dist = points[i][0];

int y_dist = points[i][1];

x_dist *= x_dist;

y_dist *= y_dist;

double dist_val = sqrt((double)x_dist + y_dist);

cout << i << " has distance of " << dist_val << " from origin!" << endl;

pq.push({dist_val, i});

}

  

// Pop heap and append to ans vector

int count = 0;

while(count < k) {

cout << pq.top().second << " is the next closest point" << endl;

ans.push_back(points[pq.top().second]);

pq.pop();

count++;

}

  

return ans;

}

};
```

## Approach 2- Max Heap
This is the efficient approach.
Here, instead of using a min heap, we will use a max heap and whenever the size of the heap becomes greater than k, we pop the top element. This makes sure that points that become irrelevant to us are removed and only the k closest points are kept in consideration.

`Note: The only reason this method is accepted in this problem is because the points can be in any order. If that was not the case, min heap would've been the solution`.

### Code 
``` cpp
class Solution {

public:

vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

priority_queue <pair<int, int>> pq; // Max heap

vector<vector<int>> ans;

  

// Add the points to the min heap

int n = points.size();

for(int i = 0; i < n; i++) {

int dist_val = (points[i][0] * points[i][0]) + (points[i][1] * points[i][1]);

pq.push({dist_val, i});

  

// Make sure size of heap does not exceed k

if(pq.size() > k) pq.pop();

}

  

// Pop from heap and append to ans vector

int count = 0;

while(count < k) {

ans.push_back(points[pq.top().second]);

pq.pop();

count++;

}

  

return ans;

}

};
```