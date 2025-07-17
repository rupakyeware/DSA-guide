Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Linked List]]

In this [[DSA]] problem, given the `head` of a singly linked list, return _the middle node of the linked list_.

If there are two middle nodes, return **the second middle** node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [3,4,5]
**Explanation:** The middle node of the list is node 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

**Input:** head = [1,2,3,4,5,6]
**Output:** [4,5,6]
**Explanation:** Since the list has two middle nodes with values 3 and 4, we return the second one.

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

## Approach 
It's a simple [[2 pointers]] approach where we will use a fast and slow pointer. When the fast pointer has traversed n nodes, the slow pointer will have traversed exactly n/2 nodes. So if the fast is at the end, slow will be at the middle.

We just have to take care of how we handle the case of even and odd pointers. Go through the code to understand that.

### Code 
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
    ListNode* middleNode(ListNode* head) {
        ListNode* s = head;
        ListNode* f = head;
        while(f != nullptr) {
            if(f->next != nullptr) {
                if(f->next->next != nullptr) f = f->next->next;
                else return s->next;
            }
            else return s;

            s = s->next;
        }

        return s;
    }
};
```