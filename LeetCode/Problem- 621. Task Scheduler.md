Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[priority queue]], [[Queue]]

In this [[DSA]] problem, you are given an array of CPU `tasks`, each labeled with a letter from A to Z, and a number `n`. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of **at least** `n` intervals between two tasks with the same label.

Return the **minimum** number of CPU intervals required to complete all tasks.

**Example 1:**

**Input:** tasks = ["A","A","A","B","B","B"], n = 2

**Output:** 8

**Explanation:** A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.

**Example 2:**

**Input:** tasks = ["A","C","A","B","D","B"], n = 1

**Output:** 6

**Explanation:** A possible sequence is: A -> B -> C -> D -> A -> B.

With a cooling interval of 1, you can repeat a task after just one other task.

**Example 3:**

**Input:** tasks = ["A","A","A", "B","B","B"], n = 3

**Output:** 10

**Explanation:** A possible sequence is: A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B.

There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.

**Constraints:**

- `1 <= tasks.length <= 104`
- `tasks[i]` is an uppercase English letter.
- `0 <= n <= 100`

## Approach
This was a very fun problem but also a bit challenging at the same time. I wasn't able to come up with the intuition to solve this problem by myself. 

==First thing to note is that we have to pick the characters with the largest frequencies at any given time so that we'll have options to work with in the cool down period. ==

==Next thing to note is that we don't care about the labels themselves. Just the frequencies associated with those labels. This was the main part of the problem that made it much easier to solve. ==

We'll first create a [[Frequency Map]] of the characters and add those frequencies to a max heap. We want to have some way of tracking the cool down period. So we'll use a queue for that. 

Every time we pop the largest element from the heap, we reduce it's count by one and create a pair of it with the time when it can be pushed back into the heap (time + n). 

Before we pop an element from the heap, we want to check if the element at the front of the queue can be added back to the heap. If it can, we push it back to the heap and pop it from the queue.

This keeps on going until both the heap and queue are empty/

### Code 
``` cpp
#define endl '\n'

  

class Solution {

public:

int leastInterval(vector<char>& tasks, int n) {

// create a frequency map of characters in the tasks array

unordered_map<int, int> frq_map;

for(char c: tasks) frq_map[c-'A']++;

  

// add frq of chars to a max heap

priority_queue<int> max_heap;

for(auto item: frq_map) max_heap.push(item.second);

  

// create a queue to handle scheduling

queue<pair<int, int>> q;

int time = 0;

  

// keep moving time ahead until both, heap and queue, aren't empty

while(!max_heap.empty() || !q.empty()) {

// check if element at front of queue can be added back to heap

if(!q.empty() && time >= q.front().second) {

auto item = q.front();

max_heap.push(item.first);

q.pop();

}

  

time++; // move time ahead

if(max_heap.empty()) continue; // no element to pop from heap

int next = max_heap.top();

max_heap.pop();

if(next - 1 > 0) { // if task is not finished yet, add it to queue

q.push({next - 1, time+n});

}

}

  

return time;

}

};
```