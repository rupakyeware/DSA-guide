Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Linked List]]

In this [[DSA]] problem, you are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

_Reorder the list to be on the following form:_

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

**Input:** head = [1,2,3,4]
**Output:** [1,4,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,5,2,4,3]

## Approach 
I solved this problem by myself after looking at the hints and trying to figure it out. I took some help form Claude in the end because I was extremely close and then I got confused. But hey, I was able to spot a pattern from [[Problem- 206. Reverse Linked List]]. 

So there are 3 main steps to this problem- 
1. Find the length of the array and then find the point of division (length + 1) / 2
2. Sever the link at the point of division to make them 2 LLs and reverse the 2nd LL
3. Alternately link elements form the 1st and 2nd LL together. 

Let's see the steps individually
For the first step, we just have to iterate from start to end while keeping a count of the number of elements we have iterated.

For the second step, we keep [[2 pointers]], prev and curr and iterate while keeping a count again until count == mid. Then we sever the link between prev and curr.
Then we reverse the second LL, whose head is pointed to by curr at that point. 

For the 3rd and final step, we have 2 heads, head and prev.
We'll also need two temporary variables, temp and temp2.
temp will be used to get the head's original next element and temp2 for prev's original next element. We make head point to prev. Prev point to temp. Then move head to temp and prev to temp2.
We keep doing this until head and prev are both valid.

We now have a reordered linked list. 

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

void reorderList(ListNode* head) {

// Find number of nodes in ll

int length = 0;

ListNode *temp = head;

while(temp) {

length++;

temp = temp->next;

}

  

// Split the ll from mid

int mid = (length + 1) / 2;

int count = 1;

ListNode* prev = nullptr;

ListNode* curr = head;

while(count <= mid) {

prev = curr;

curr = curr->next;

count++;

}

prev->next = nullptr;

  

// Reverse the second ll

prev = nullptr;

while(curr) {

temp = curr->next;

curr->next = prev;

prev = curr;

curr = temp;

}

  

ListNode* temp2;

// Merge the 2 lls

while(head && prev) {

temp = head->next;

temp2 = prev->next;

  

head->next = prev;

prev->next = temp;

  

head = temp;

prev = temp2;

}

}

};
```