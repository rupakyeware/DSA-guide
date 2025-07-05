Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Binary Search]], [[priority queue]]

In this [[DSA]] problem, the **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

**Input**
```
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
```
**Output**
[null, null, null, 1.5, null, 2.0]

**Explanation**
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0

**Constraints:**

- `-105 <= num <= 105`
- There will be at least one element in the data structure before calling `findMedian`.
- At most `5 * 104` calls will be made to `addNum` and `findMedian`.

**Follow up:**

- If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
- If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

## Approach 1- Brute force
We have to maintain a sorted array at all times. Instead of inserting and then sorting every time, we can just find the position of the  [[lower_bound]] of the array for the number we want to insert and then insert the number one element before it. Then the logic to find the median is going to be the same. If odd return the middle element and if even, return the mean of the two middle elements. 

But the time complexity of this method is a lot. Inserting all the elements in an ordered manner will taken O(nlog(n)) in total while finding the medians will be done in n. So that's O(nlog(n)). 

Actually now that I think about it, insertion is closer to n than nlog(n) because after inserting an element, all the next elements also have to be shifted one place ahead so that'll take O(n). So the total time is closer to `O(n^2).`

### Code 
``` cpp
class MedianFinder {

public:

vector<int> nums;

  

MedianFinder() {

}

void addNum(int num) {

int pos = lower_bound(nums.begin(), nums.end(), num) - nums.begin();

nums.insert(nums.begin() + pos, num);

}

double findMedian() {

int size = nums.size();

if(size % 2) {

int mid = (size - 1) / 2;

return (double) nums[mid];

}

else {

int mid1 = (size - 1) / 2;

int mid2 = size / 2;

  

return ((double) nums[mid1] + nums[mid2]) / 2;

}

}

};

  

/**

* Your MedianFinder object will be instantiated and called as such:

* MedianFinder* obj = new MedianFinder();

* obj->addNum(num);

* double param_2 = obj->findMedian();

*/
```

## Approach 2- Using 2 heaps
This approach from Neetcode is actually pretty genius but also simple at the same time. Since we want to find the median at any given time, we want to divide the array into 2 parts as equally as we can and then access the last element of the first part, and the first element of the second part.

To achieve this in O(1) time without having to iterate through the array, we can use 2 heaps.
For the 1st subarray we'll use a maxHeap and for the second one, we'll use a minHeap.
But, we also want to balance the heaps as the size of either of them cannot be greater than the size of the other heap + 1. This is because we need to keep moving the point of division of the array based on the elements that come in. 

So if the maxHeap is greater, we just pop from it and push that element into the minHeap and vice versa. This is equivalent to moving the line of division left or right by one. And we keep doing this until the heaps are balanced.

To get the median, if the heaps are equal, it means the array has an even number of elements and we need to return the mean of the 2 top elements. But if it's an odd number of elements, then we need to return the top element of the heap having a greater number of elements. 

### Code 
``` cpp
class MedianFinder {

public:

priority_queue<int> maxHeap; // left subarr

priority_queue<int, vector<int>, greater<int>> minHeap; // right subarr

  

MedianFinder() {

}

void addNum(int num) {

if(maxHeap.empty() || num <= maxHeap.top()) maxHeap.push(num);

else minHeap.push(num);

  

// Balance the heaps if needed

while(maxHeap.size() > (minHeap.size() + 1)) {

int top = maxHeap.top();

maxHeap.pop();

minHeap.push(top);

}

  

while(minHeap.size() > (maxHeap.size() + 1)) {

int top = minHeap.top();

minHeap.pop();

maxHeap.push(top);

}

}

double findMedian() {

int maxSize = maxHeap.size();

int minSize = minHeap.size();

  

if(maxSize == minSize) {

int top1 = maxHeap.top();

int top2 = minHeap.top();

return ((double) top1 + top2) / 2;

}

else {

if(maxSize > minSize) return (double) maxHeap.top();

else return (double) minHeap.top();

}

}

};

  

/**

* Your MedianFinder object will be instantiated and called as such:

* MedianFinder* obj = new MedianFinder();

* obj->addNum(num);

* double param_2 = obj->findMedian();

*/
```