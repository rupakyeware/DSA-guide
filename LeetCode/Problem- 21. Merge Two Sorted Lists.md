Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Linked List]]

In this [[DSA]] problem, you are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

**Input:** list1 = [1,2,4], list2 = [1,3,4]
**Output:** [1,1,2,3,4,4]

**Example 2:**

**Input:** list1 = [], list2 = []
**Output:** []

**Example 3:**

**Input:** list1 = [], list2 = [0]
**Output:** [0]

## Approach
This was a pretty easy problem. I was on the right track by myself but I needed a bit of a nudge in the right direction.
We create a dummy node and initialize it with 0.
Then we just have to compare the values of l1 & l2 and keep appending and incrementing the smaller of the 2 until one of them gets fully traversed.
Then we just append the entire pending array of the 2 to the new linked list we formed.
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

ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

ListNode dummy(0);

ListNode* p = &dummy;

  

while(list1 && list2) {

if(list1->val <= list2->val) {

p->next = list1;

list1 = list1->next;

}

else {

p->next = list2;

list2 = list2->next;

}

p = p->next;

}

  

if(list1) {

p->next = list1;

}

else if(list2) {

p->next = list2;

}

  

return dummy.next;

}

};
```

