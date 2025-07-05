Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Linked List]], [[Floyd's Tortoise & Hare]], [[2 pointers]], [[Unordered Set]]

In this [[DSA]] problem, given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** false
**Explanation:** There is no cycle in the linked list.

## Approach 1- Using a hashset 
This is the approach I thought of. I stored the pointers of the visited nodes in a [[Unordered Set]] and every time I traversed a new node I checked if we've already visited it before. If we have, then I return else.
Else if we haven't seen it before we insert it into the set. Then if the next node is null we return false else we move to the next node.
This approach takes O(nlog(n)) time and O(n) space.

### Code 
```
/**

* Definition for singly-linked list.

* struct ListNode {

* int val;

* ListNode *next;

* ListNode(int x) : val(x), next(NULL) {}

* };

*/

class Solution {

public:

unordered_set<ListNode*> isVisited;

bool hasCycle(ListNode *head) {

if(!head) return false;

  

if(isVisited.find(head) != isVisited.end()) return true;

if(head->next == nullptr) return false;

isVisited.insert(head);

return hasCycle(head->next);

}

};
```

## Approach 2- [[Floyd's Tortoise & Hare]] algorithm with [[2 pointers]]
This is a very cool approach. We initialize two pointers, slow and fast. The slow with move one step at a time while the fast will move 2.
If the fast reaches a null ptr then we return false;
But if at any point, they meet, which they definitely will if there exists a cycle, we return true;

### Code 
```
/**

* Definition for singly-linked list.

* struct ListNode {

* int val;

* ListNode *next;

* ListNode(int x) : val(x), next(NULL) {}

* };

*/

class Solution {

public:

unordered_set<ListNode*> isVisited;

bool hasCycle(ListNode *head) {

if(!head) return false;

  

if(isVisited.find(head) != isVisited.end()) return true;

if(head->next == nullptr) return false;

isVisited.insert(head);

return hasCycle(head->next);

}

};
```
