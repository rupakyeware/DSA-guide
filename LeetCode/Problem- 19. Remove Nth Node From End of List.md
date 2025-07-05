Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Linked List]], [[2 pointers]]

In this [[DSA]] problem, given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

**Input:** head = [1,2,3,4,5], n = 2
**Output:** [1,2,3,5]

**Example 2:**

**Input:** head = [1], n = 1
**Output:** []

**Example 3:**

**Input:** head = [1,2], n = 1
**Output:** [1]

## Approach 1- Calculate total length and delete the len-n+1th node 
This is the approach I came up with. It's pretty simple to understand. We first find the total length of the LL. Then we find the number of the target node to delete with the formula 
`total_length - n + 1`. 
Then we iterate through the LL again while incrementing a counting variable. Once count == target, we delete the node using 2 pointers.
But if target = 1, we just move the head forward.
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

ListNode* removeNthFromEnd(ListNode* head, int n) {

// Find the total length of the LL

int len = 0;

ListNode* curr = head;

while(curr) {

len++;

curr = curr->next;

}

  

// Delete the node

int count = 0;

int node_to_delete = len - n + 1;

ListNode *prev = nullptr;

curr = head;

while(curr) {

count++;

if(count == node_to_delete) {

if(node_to_delete == 1) {

head = head->next;

}

else {

cout << "prev:" << prev->val << " curr:" << curr->val << endl;

prev->next = curr->next;

curr->next = nullptr;

}

break;

}

else {

prev = curr;

curr = curr->next;

}

}

  

return head;

}

};
```

## Approach 2- Greedy Two Pointers 
This method is so damn cool. It's kind of like if you wanted to walk towards a wall with your eyes closed, you'd hold your hands out to stop a fixed distance before the wall.
We use something like that here.
We use 2 pointers, first and second.
First will be initialized to head and then shifted n spaces ahead.
Then we'll create a dummy node and initialize second to it. 
Then we go on moving both the pointers one step ahead until first becomes null.
So now, we have actually perfectly stopped at the node BEFORE the node we want to delete and that's exactly what we want in deletion.
Then we just delete the next node using `second->next = second->next->next`
And this also works very well with a single element LL because of the dummy node and we don't have to add an extra condition like I did in my code in the approach above this.
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

ListNode* removeNthFromEnd(ListNode* head, int n) {

ListNode* first = head;

int count = 0;

while(count < n) {

count++;

first = first->next;

}

  

ListNode dummy(0, head);

ListNode* second = &dummy;

while(first) {

first = first->next;

second = second->next;

}

  

second->next = second->next->next;

return dummy.next;

}

};
```