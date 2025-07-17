Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Linked List]]

In this [[DSA]] problem, you are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

**Input:** l1 = [2,4,3], l2 = [5,6,4]
**Output:** [7,0,8]
**Explanation:** 342 + 465 = 807.

**Example 2:**

**Input:** l1 = [0], l2 = [0]
**Output:** [0]

**Example 3:**

**Input:** l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**Output:** [8,9,9,9,0,0,0,1]

## Approach
We approach this literally like elementary school addition since the input lists are reversed and the output list also has to be reversed. We'll have to keep the carry in mind.
Initialize the carry to 0 and [[2 pointers]], p1 and p2 to the start of lists 1 and 2 respectively.
Then we calculate the sum of both the numbers and the carry, set the carry as sum / 10, create a new node and then point current, which will be initialized pointing to a dummy node, to the new node. Then we move current, and the 2 pointers one step ahead each.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(-1);
        int carry = 0;

        ListNode* curr;
        curr = head;
        while(l1 != nullptr && l2 != nullptr) {
            int sum = l1->val + l2->val + carry;
            curr->next = new ListNode(sum % 10);
            carry = sum / 10;

            curr = curr->next;
            l1 = l1->next;
            l2 = l2->next;
        }

        if(l1 != nullptr) {
            while(l1 != nullptr) {
                int sum = l1->val + carry;
                curr->next = new ListNode(sum % 10);
                carry = sum / 10;     

                l1 = l1->next;
                curr = curr->next;
            }
        }
        else if(l2 != nullptr) {
            while(l2 != nullptr) {
                int sum = l2->val + carry;
                curr->next = new ListNode(sum % 10);
                carry = sum / 10;     

                l2 = l2->next;
                curr = curr->next;
            }
        }

        if(carry) {
            curr->next = new ListNode(carry);
        }

        return head->next;
    }
};
```