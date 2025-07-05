Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Linked List]], [[Recursion]], [[2 pointers]]

In this [[DSA]] problem, given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**
![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)
**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**
![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)
**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

There are 2 key things that we have to do. 
1. Reverse the links of each node 
2. Return the new head (last node of original list) so find a way to keep a track of it
## Approach 1: [[Recursion]]
In this approach, we will essentially traverse to the end of the linked list, return the last node as the newHead, and then reverse the links. It's a little hard to explain in text.

This approach has an O(n) time complexity as well as an O(n) space complexity.

### Code 
```
class Solution {

public:

ListNode* reverseList(ListNode* head) {

if(!head) return nullptr;

  

ListNode* newHead = head;

if(head->next) {

newHead = reverseList(head->next);

head->next->next = head;

}

head->next = nullptr;

  

return newHead;

}

};
```

## Approach 2- Using 2 pointers iteratively
This approach is better than the previous one as the time complexity is the same but the memory complexity is O(1)

Here, we will use 2 pointers, prev and curr. The curr will point to the curr node. And we will keep iterating it ahead until it's not null. The prev will point to the node before it.
While curr is valid, we will make a temporary variable to store curr->next.
Then we will change curr->next to prev, move the prev ahead and move curr to temp;
It's very simple to understand as well as implement. Just make sure you're using the right variables in the while loop (curr and head in the right places).

### Code 
```
/**

* Definition for singly-linked list.

* struct ListNode {

* int val;

* ListNode *next;

* ListNode() : val(0), next(nullptr) {}

* ListNode(int x) : val(x), next(nullptr) {}

* ListNode(int x, ListNode *next) : val(x), next(next) {}

* };

*/

class Solution {

public:

ListNode* reverseList(ListNode* head) {

ListNode* prev = nullptr;

ListNode* curr = head;

while(curr) {

ListNode* temp = curr->next;

curr->next = prev;

prev = curr;

curr = temp;

}

  

return prev;

}

};
```
