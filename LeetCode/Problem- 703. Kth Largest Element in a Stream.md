Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[priority queue]]

In this [[DSA]] problem, you are part of a university admissions office and need to keep track of the `kth` highest test score from applicants in real-time. This helps to determine cut-off marks for interviews and admissions dynamically as new applicants submit their scores.

You are tasked to implement a class which, for a given integer `k`, maintains a stream of test scores and continuously returns the `k`th highest test score **after** a new score has been submitted. More specifically, we are looking for the `k`th highest score in the sorted list of all scores.

Implement the `KthLargest` class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of test scores `nums`.
- `int add(int val)` Adds a new test score `val` to the stream and returns the element representing the `kth` largest element in the pool of test scores so far.

**Example 1:**

**Input:**  
`["KthLargest", "add", "add", "add", "add", "add"] 
`[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]`

**Output:** `[null, 4, 5, 5, 8, 8]`

**Explanation:**

```
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);  
kthLargest.add(3); // return 4  
kthLargest.add(5); // return 5  
kthLargest.add(10); // return 5  
kthLargest.add(9); // return 8  
kthLargest.add(4); // return 8
```

**Example 2:**

**Input:**  
`["KthLargest", "add", "add", "add", "add"]  `
`[[4, [7, 7, 7, 7, 8, 3]], [2], [10], [9], [9]]

**Output:** `[null, 7, 7, 7, 8]`

**Explanation:**

```
KthLargest kthLargest = new KthLargest(4, [7, 7, 7, 7, 8, 3]);  
kthLargest.add(2); // return 7  
kthLargest.add(10); // return 7  
kthLargest.add(9); // return 7  
kthLargest.add(9); // return 8
```

**Constraints:**

- `0 <= nums.length <= 104`
- `1 <= k <= nums.length + 1`
- `-104 <= nums[i] <= 104`
- `-104 <= val <= 104`
- At most `104` calls will be made to `add`.

## Approach 
Since we want to find the kth highest element at any given time, it makes sense to use a min-heap of size k. When we want to add an element, we push it to the priority queue. If the size of the priority queue is greater than k, we pop the top most element, as it's not relevant to us anymore and then we return the new top element. 

### Code 
``` cpp
class KthLargest {

public:

priority_queue<int, vector<int>, greater<int>> pq; // min heap

int k_highest;

  

KthLargest(int k, vector<int>& nums) {

k_highest = k;

for(int i: nums) {

add(i);

}

}

int add(int val) {

pq.push(val);

if(pq.size() > k_highest) pq.pop();

  

return pq.top();

}

};

  

/**

* Your KthLargest object will be instantiated and called as such:

* KthLargest* obj = new KthLargest(k, nums);

* int param_1 = obj->add(val);

*/
```